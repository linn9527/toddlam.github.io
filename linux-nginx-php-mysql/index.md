# LNMP 基础环境搭建


<!--more-->
## 基础环境

### 系统

Debian 3.16.0-4-686-pae #1 SMP Debian 3.16.7-ckt20-1+deb8u1 (2015-12-14) i686 GNU/Linux

### 软件

php7.0.4, nginx1.9.12, mysql或mariadb

## 安装

### 更新软件包

    （只是每天都干，顺手）

`#apt-get update` & `#apt-get upgade`

### 手动安装（Debian里面的包不是最新的，全部手工安装）

#### 安装 Nginx （[官网](http://nginx.org/)）

1. 下载安装包 `wget http://nginx.org/download/nginx-1.9.12.tar.gz`

2. 解压缩 `tar -zxvf nginx-1.9.12.tar.gz`

3. 进入目标目录 `cd ./nginx-1.9.12`

4. 配置 `./configure --prefix=/usr/local/nginx` 

    报错 找不到 cc 编译器，

5. 装 gcc `#apt-get install gcc`，再执行第四步

    报错 `./configure: error: the HTTP rewrite module requires the PCRE library.` 
    
6. 需要 PCRE 库， `#apt-get install libpcre3 libpcre3-dev`，再执行第四步

    报错 `./configure: error: the HTTP gzip module requires the zlib library.`

7. 需要 zlib 库 `#apt-get install zlib1g-dev`，再执行第四步

8. 安装 `#make && make install`

    报错，找不到 make （「..「|||）
    
9. 安装 make 工具 `#apt-get install make`，再执行第八步
    
10. 安装完，先修改一下配置文件，将端口修改为8080（已装有Apache默认监听了80，避免端口冲突）

11. 执行 `/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf` 然后访问 `localhost:8080` 就可以看到欢迎页面了

![Welcome to Nginx](https://gitee.com/toddlam/Colony/raw/master/pic/2020-05-10/E4Xpkl.png "Welcome to Nginx")

### 安装 Mariadb （[官网](http://mariadb.org/)）

> Nginx 和 PHP ，两者其实在运行过程中与 mysql 基本没有什么特别关系，所以按照最方便的方式安装即可

`#sudo apt-get install mariadb-client mariadb-server` 

测试 `mysql -u root -p` 输入密码 OK

### 安装 PHP7 （[官网](http://php.net/))

1. 下载官方最新版安装包 `git clone http://git.php.net/repository/php-src.git` 比较慢

2. 进入目录，里面有 travis 集成的脚本，可自行查看安装过程（不用做）

3. 执行 `./buildconf` 缺什么软件装什么软件～～不一一细说

4. configure 操作，详情可参考2

```configure
./configure --prefix=/usr/local/php7 \
--with-config-file-path=/usr/local/php7/etc \
--with-mcrypt=/usr/include \
--with-mysqli \
--with-pdo-mysql \
--with-gd \
--with-iconv \
--with-zlib \
--enable-xml \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-inline-optimization \
--enable-mbregex \
--enable-fpm \
--enable-mbstring \
--enable-ftp \
--enable-gd-native-ttf \
--with-openssl \
--enable-pcntl \
--enable-posix \
--enable-sockets \
--with-xmlrpc \
--enable-zip \
--enable-soap \
--with-pear \
--with-gettext \
--enable-session \
--with-curl \
--with-jpeg-dir \
--with-freetype-dir \
--enable-opcache \
--with-fpm-user=www \
--with-fpm-group=www \
--enable-bcmath \
--enable-shmop \
--enable-mbstring \
--with-mhash
```

    报错，`error: xml2-config not found. Please check your libxml2 installation.`
  
5. 单装了 libxml2 是不行的，需要安装 libxml2-dev（Debian是这个包）`sudo apt install libxml2 libxml2-dev `

        报错，`Cannot find OpenSSL's <evp.h>`

6. 安装 `apt-get upgrade libssl-dev`

        报错，`error: Cannot find OpenSSL's libraries`

7. 添加 libdir 配置 `--with-libdir=xxxx-linux-gnu`

        报错，`error: Please reinstall the libcurl distribution`

8. 安装 `sudo apt-get install libcurl4-openssl-dev`

9. make && make install

## 总结

手动编译安装的过程，很多时候的报错其实也就是三种情况

1. 库缺失，可以通过安装缺失的lib解决
2. 安装后还缺，可以通过configure阶段，修改相关路径解决
3. 头文件缺失，可以通过相关的lib对应的dev包解决，因为一般非dev包不包含 `*.h` 的文件
