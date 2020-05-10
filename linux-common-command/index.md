# Linux常用的命令


<!--more-->


## 随便记

1. `uname -a` 显示当前系统信息 `-a` 表示 `all`

2. `pwd` 显示当前路径
 
3. `chattr +i / -i` 增加/去除 强制不能修改文件，啥都不能改，常用在 /etc/resolv.conf 上，还有其它用法 `lsattr a.txt`是对应的查看

4. `chmod` 修改权限

5. `chown` 修改拥有者

6. `docker rmi $(docker images | grep "none" | awk '{print $3}')` docker 删除镜像

7. `dig www.baidu.com @114.114.114.114` 验证DNS

8. `man -t man | open -f -a Preview` 用 PDF 来查看 man 文档 贼6

9. `caffeinate -t 3600` Mac 不休眠 一小时

### 想加再加

