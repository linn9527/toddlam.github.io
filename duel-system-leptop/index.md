# 笔记本上双系统安装


linux 的好 都懂，windows 的好 也都懂，小孩子才做选择题「..「 
<!--more-->

## 为啥要装双系统

1. 原来用着的 Windows 用着越来越慢卡，本来就准备重装
2. 作为程序狗，Linux 可能在某些方面比较友好，尝试在日常使用
3. QQ聊天、IE网银等无法直接放弃

![守护的笑容](https://gitee.com/toddlam/Colony/raw/master/pic/2020-05-08/SIcXQF.jpeg)


-------

    其他方案

- [x]两台电脑 **没钱**
- [x]wine/虚拟机 **电脑配置不够，用着会炸毛**


## 系统选择

Win：win7/win8/win8.1

    对于新的 win8 和 8.1 实在是看不惯，选 win7

Linux：Ubuntu/Archlinux/Debian/Fedora/CentOs/LFS

    Ubuntu 和 Debian 都在桌面环境比较成熟，一键安装没毛病
    Fedora 和 CentOs 相对服务端使用更多，包一般比较旧
    LFS 系统玩你还是你玩系统
    Archlinux 安装相对比较麻烦，但都是入门需要涉及的系统知识，滚动升级，装好就能一直滚，丰富的文档和AUR
    所以选兼顾学习和桌面，选 Archlinux ，也适合新手

## 安装介质

- 笔记本电脑：4G/512机械
- U盘一个
- win7-32镜像、Archlinux镜像各一个
- UltralISO软件/dd4windows

## 分区计划

- Windows只分一个（256G），NTFS分区同时可以被Archlinux访问，足够了
- Archlinux分了两个，/（64G）和/home（128G），可以独立分多几个，也可以预留一部分空间，需要再加

      整体大概剩余 64G 空间，后续扩充的时候再使用

## Windows7 系统安装

1. 官网下载、安装UltralISO

2. 打开win7镜像，刻录到U盘

3. 重启，安装（注意硬盘分区，可以直接在安装过程分区）

4. 一步一步跟着做，安装完成后注册，更新。（此处支持正版）

5. 打开软件卸载—>打开或关闭windows功能，删除不需要的功能

6. 安装软件：QQ，IE（10 or 9就好），WPS Office，mupdf，uTorrent。

      win7 的安装十分简单

## Archlinux 系统安装

> 装好win7后，在win7下下载`dd for windows`，将Archlinux镜像写入U盘
  
    PS: 用UNetbootin好像也是可以的，用UltralISO刻录的启动不了
    而量产用的镜像能启动，但是只是能启动而已，也可能是我操作错误。


1. 分区：`cfdisk /dev/sda`，/、/home，没有Swap

2. 文件系统：`mkfs.ext4 /dev/sdaX`，文件系统直接选 ext4 

3. 挂载：`mount /dev/sdaX /mnt`，X为根目录分区；`mkdir /mnt/home`，需要先新建Home目录，再挂载；`mount /dev/sdaY /mnt/home`，Y为Home目录分区

4. 修改镜像列表：`vim /etc/pacman.d/mirrorlist`，校园网用户可以加上上海交大的源，那个速度十分不错，其他用户可以将163等几个国内的源放到列表最前面

5. 安装基本系统：`pacstrap -i /mnt base base-devel`，一般装上这两个就OK，喜欢自定义可以选择自己需要的包安装，在base里包含了基本系统使用的工具，base-devel包含了开发需要的一些工具，装好后`#pacman -S XXX`查一下就知道啦

6. 生成fstab：`genfstab -U -p /mnt >> /mnt/etc/fstab` 生成一次就行，之后最好只用手动修改

7. 切换系统：`arch-chroot /mnt /bin/bash`，使用bash进入系统，若没有最后的参数，默认为sh

8. Locale：`nano /etc/locale.gen`，将中文和英文前面的#去掉，然后`locale-gen`一下～

9. Time Zone：`ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`

10. Hostname：`echo Fengzi >/etc/hostname`

11. Password：`passwd`

12. 安装Grub：`pacman -S grub os-prober` os-prober是为了让Grub能搜索到刚刚装上的win7，`grub-install --target=i386-pc --recheck /dev/sda` 将grub安装到硬盘上，`grub-mkconfig -o /boot/grub/grub.cfg` 配置文件在 `/boot/grub/gurb.cfg`

13. 重启：`exit`，`umount -R /mnt`，`reboot`

14. 添加用户：`useradd -m -g users psycho`，`passwd psycho`

15. 安装ALSA：声卡驱动，`#pacman -S alsa-utils`，之后可以在终端`alsamixer`，在下面显示为MM的为静音，一般条Master和PCM就OK了

16. 安装显卡驱动：A卡和Intel的直接开源驱动无异意，N卡想要更好的体验就去装闭源的，由于我电脑属于垃圾电脑，对我来说方便就好，开源的分别为`xf86-video-ati xf86-video-intel xf-video-nouveau`，不清楚状况的就`#pacman -S xf86-video-vesa`～

17. 安装触摸板驱动：`#pacman -S xf86-input-synaptics`

18. 安装X11：`#pacman -S xorg-xinit xorg-server`，工具包什么的就不需要装了

19. 安装桌面：目前功能完整多样的桌面环境有Xfce（算么？）、Gnome、KDE、E17（18？）、LXDE（gtk or qt）、Mate（Gnome2的一个fork）、Cinnamon（Gnome3的一个fork）等，都是基于gtk or qt的，除了E17自成一家。

### 其他桌面环境

Xfce：简单，装过，资源消耗低，功能该有的都有，像瘦子，没饿死那种

Gnome：2的话有Mate，3的话有Cinnamon，是Mint的默认桌面，都比较漂亮！

LXDE：听说有gtk和qt两个版本，相比Xfce，更倾向与这货，资源消耗同样较低，安装`#pacman -S lxde`，想拆分这个group来安装也可以～

KDE：漂亮，功能齐全，特效多，插件多，不过同样资源消耗大，慢（和win7应该差不多吧），最简安装就好`#pacman -S kdebase kdebase-workspace`，其余软件，按需安装

E17：好像是挺漂亮的，听说资源消耗还挺低。。没装过

Mate：虽然有人说里面的都是一对过时的代码，不过。。一般人管他代码是怎样的。。用起来舒服就好，安装需要先修改`/etc/pacman.conf`，在里面加入Mate的源，`Server=http://packages.mate-desktop.org/repo/archlinux/$arch`，然后更新`#pacman -Syy`，安装`#pacman -S mate`

Cinnamon：这货在官方源里就有，安装`#pacman -S cinnamon`

其他类似平铺式的桌面不适合一般用户。。。（说的就是我）

> Enjoy!


