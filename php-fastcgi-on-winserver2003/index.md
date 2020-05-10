# Windows Server2003开启IIS和PHP环境


难以理解为什么现在还有人用win去跑PHP
<!--more-->

## 必要工具

- [扩展下载地址](http://www.iis.net/extensions/fastcgi) 

- [IIS的英文文档](http://www.iis.net/learn/application-frameworks/install-and-configure-php-applications-on-iis/using-fastcgi-to-host-php-applications-on-iis-60)

- [PHP window下载](http://windows.php.net/download)

## 安装IIS的FastCGI扩展

1. 打开下载扩展的链接，选择自己的windows2003的版本32位还是64位

2. 如果不知道自己的服务器多少位的，可以打开运行，执行winmsd.exe看看

3. 下载相应msi文件下载，双击运行一直确定就可以了！

## 安装PHP

1. 到PHP下载的网站选择线程不安全的版本进行下载安装。注意版本，PHP>=5.3的版本才可以用FastCGI。

2. 安装php，直接解压，不多说，配置Path，验证时打开CMD输入`php -v`就可以看到php当前版本号。

3. 修改PHP配置文件，一般PHP解压文件根目录会有两个配置文件，一个用于开发，一个用于生产，随便复制一个，重命名为php.ini。打开，里面一般需要配置几个地方，一个是时区`data.timezone=Asia/Shanghai`，一个是扩展的目录`extension_dir="C:\PHP\ext"`，剩下的就是打开扩展了，将需要打开的扩展前面的`;`去掉。

## 是配置FastCGI扩展

1. 从CMD进入目录`%windir%\system32\inetsrv`，

2. 执行`cscript fcgiconfig.js -add -section:"PHP" -extension:php -path:"C:\PHP\php-cgi.exe"`，将路径换为PHP解压路径

3. 打开IIS6.0管理器，右击`网站`选择属性，点击根目录的tab，下方有个`管理`，如果之前配置过了，可以找到后缀为`.php`的那一栏双击，没有则新建。

4. 进入后点击浏览，到目录`%windir%\system32\inetsrv`，找到`fcgiext.dll`文件，选择，扩展名自然就是`.php`，下面有动词什么的就是HTTP的六个方法，选择全部就可以了。

5. 接下来需要配置`fcgiext.dll`，在同一目录下找到文件`fcgiext.ini`，在`[TYPE]`下添加`php=PHP`，回车然后。。继续添加`[PHP]`，回车然后。。继续添加`ExePath=c:\php\php-cgi.exe`，路径换回解压的路径，这样，重启应用程序池，接下来测试

> OoO 搞定

