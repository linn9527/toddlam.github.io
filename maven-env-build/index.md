# Maven环境搭建


<!--more-->

## 前言

很久没写Java，忽然想搭建个环境「..「

直接刚

## 环境搭建（只有Mac）

### Java

安装Java很简单，直接Oracle官网下载，最新是1.8，直接最新版

    PS：此处可以用brew cask安装，具体自行google

安装好之后，就是配置环境了，有两个环境变量， `JAVA_HOME`（没有它，Maven会报错）和 `CLASS_PATH`（Java应用运行时找包会在这找），Java目录比较难找，可以用`which`命令找下路径，一般是这样的`/Library/Java/JavaVirtualMachines/jdk1.8.0_20.jdk/Contents/Home`

然后随便打开一个跟 profile（或者rc）相关的文件，就`.bash_profile`之类的，添加

```conf
# 配置Java Home
JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_20.jdk/Contents/Home"
export JAVA_HOME
# 配置Class Path
CLASS_PATH="$JAVA_HOME/lib"
export CLASS_PATH
# 添加PATH
export PATH=$PATH:$JAVA_HOME/bin
```

### Maven

安装 Maven ，简单，`brew install maven`，

`M2_HOME`、`MAVEN_HOME` 配不配貌似没有影响

### 测试环境

`java －version` 查看Java版本

`javac` 查看Java编译命令是否可以使用

`mvn -v` 查看Maven版本

## 新建项目测试

打开命令行

在当前目录下新建Maven Web项目

```Bash
mvn archetype:create	// 创建命令
 -DgroupId=com.eata	// 设置GroupId
 -DartifactId=Newone	// 设置ArtifactId
 -DarchetypeArtifactId=maven-archetype-webapp // 这是Maven的创建Web项目的模版，此外还有很多模版，可自行搜索
```

创建完后，会有一个新文件夹`Newone`出现

执行`mvn`，然后会显示出错

>No goals have been specified for this build. You must specify a valid lifecycle phase or a goal in the format <plugin-prefix>:<goal> or <plugin-group-id>:<plugin-artifact-id>[:<plugin-version>]:<goal>. Available lifecycle phases are: validate, initialize, generate-sources, process-sources, generate-resources, process-resources, compile, process-classes, generate-test-sources, process-test-sources, generate-test-resources, process-test-resources, test-compile, process-test-classes, test, prepare-package, package, pre-integration-test, integration-test, post-integration-test, verify, install, deploy, pre-clean, clean, post-clean, pre-site, site, post-site, site-deploy.


报上面异常是因为缺少生命周期

对于Maven有个很重要的概念，叫生命周期（lifecycle），从上看的出错信息看来，`mvn`执行需要至少一个生命周期（因为No goals）作为参数

-----

插（maven 生命周期）

Maven 有三组主要的生命周期，default（用于构建应用），clean（用于清除上一次构建的数据），site（用于生成应用的站点文档，然而里面跟应用构建没有关联），这三组生命周期调用不会产生交集

- clean 周期最简单，里面只有三个小周期，一般直接 `mvn clean`

- site 周期跟 clean 差不多，有四个，一般也是直接用`mvn site`

- default 周期关系到整个应用的构建过程，在不细分的情况下（copy），简单（按顺序）分为`compile`（编译主代码到主输出目录），`test`（执行测试用例），`package`（创建jar包），`install`（将项目输出构件安装在本地仓库），`deploy`（将项目输出构件部署到远程仓库）

-----


接下来执行`mvn deploy`，然后又会显示出错

>Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy (default-deploy) on project Newone: Deployment failed: repository element was not specified in the POM inside distributionManagement element or in -DaltDeploymentRepository=id::layout::url parameter

原因是`deploy`的生命周期是会把项目部署到远程仓库的，这里没有所谓远程仓库，所以一般构件只需要到`package`就足够了

> Enjoy!!


