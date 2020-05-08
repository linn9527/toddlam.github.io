# Appfog 使用笔记

本着娱乐的态度尝试了免费的AppFog云服务

<!--more-->

## 必要工具

Ruby 和对应的 DevKit

因为 本地使用的 af 工具是 基于 ruby 的实现，本地访问必须安装

[Ruby1.9.3](http://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-1.9.3-p545.exe?direct)

[Ruby1.9.3对应的Devkit](https://github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe)

## 注册Appfog用户（必须）

注册过程不展示，注册用户后可使用内存为512MB，刚好可以满足两个App和两个Service的创建

## 创建Java应用

选择 Java (需要512)、Aws (网传说比较块「..「)，进入App，添加Mysql数据库

## 安装本地访问工具,用af访问

### 安装Ruby:

1. 下载Ruby 1.9.3 的 Installer

根据上方的链接地址下载

2. 查看镜像 mirror (sources)

查看 Sources: `gem sources -l` 如果有，可以删掉 `gem sources --remove XXXXXX`

3. 添加 taobao 的镜像

添加Sources: `gem sources -a https://ruby.taobao.org/`

### 安装工具 af

1. 更新 gem 
`gem update --system`

2. 安装 af
`gem install af`

3. 登陆 AppFog
登录就可以使用: `af login`
根据步骤输入用户名和密码就可以开始使用af进行管理啦

### 本地访问 Appfog 数据库

> 为了本地可以访问AppFog的数据库，需要使用SSH管道(tunnel)，先安装caldecott
>
> PS: Tunnel其实只是打开一个端口，当访问本地该端口时，将信息转发到远程服务器中

安装: `gem install caldecott`

如果显示需要C Compiler 先把Devkit安装好(直接下载解压到目标路径)

进入Devkit目录，运行

`ruby dk.rb init`

`ruby dk.rb install`

搞定Devkit再安装caldecott
安装完成后: `af tunnel`

## 遇到的一些蛋碎的问题

1. 第一次安装了 Ruby2.0，gem 安装 caldecott 时缺少 Devkit

    解决: 下载对应Devkit安装

2. 安装完成后 `af tunnel` 依然显示 `caldecott not install`

    解决: 直接卸载关于2.0版本的，安装所有1.9.3相关的（原因不明，菜是原罪）

3. 运行 `af tunnel` 需要在Appfog上添加一个应用，但是内存用完了

    解决: 缩小Java应用的内存降低，默认Caldecott需要64Mb，我妥妥的给了128Mb（钱能解决的方案都不是好方案OK？）

4. 无法使用本地 HeidiSql 连接远程服务

    解决: 开启Sql服务，将Sql/bin添加到Path

> Enjoy～

