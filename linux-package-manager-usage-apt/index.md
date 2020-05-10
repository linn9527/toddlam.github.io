# Apt包管理使用


<!--more-->
为什么使用包管理？因为方便！

因为我目前的是Debian，所以介绍的是apt

## Apt 包管理工具

1. 安装 `apt-get install pkg( pkgs...)` 

2. 更新软件包列表 `apt-get update`

3. 更新软件 `apt-get upgrade` `apt-get upgrade pkg`

4. 查看依赖 `apt-cache depends pkg` 查看当前包所依赖的包，`apt-cache rdepends pkg` 查看依赖当前包的包

> Mark 一下
