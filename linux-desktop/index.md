# 自己搭建Linux桌面环境


就想自己折腾一下桌面环境搭建
<!--more-->

## 前言

> 若要让自己选择Linux桌面环境，要么就来个KDE，不需要再有什么额外的东东，办公代码一应俱全，特效也还是可以的，要么来个Cinnamon，同样挺漂亮的，相比那些节约内存的桌面环境，用着更舒服、养眼～（习惯与平铺式桌面的用户当然就另当别论啦），当然，内存应该消耗在运行的软件上，而不是在所谓漂亮的桌面上，这点我也是认同的，So，如果真需要选择一款低内存消耗的，那就Mate吧（好像也不是很低）
> 所以，我可以自己组织搭建

[假设驱动已经安装完](/duel-system-leptop)

## 需求

- 登陆管理器，DM

- 桌面需要使用鼠标

- 需要展示时间，系统托盘，任务栏，快捷起动栏

- 有桌面图标 OR 有系统监测

## 工具选择

### 系统基础软件

slim 作为登陆管理器

openbox 作为窗口管理器

feh 设置墙纸

tint2 （tint-svn）作为面板提供任务栏、系统托盘、快捷启动栏

conky （conky-lua）作为系统监测

pcmanfm 作为文件管理器（也可以做桌面）

### 应用工具

LibreOffice

Mupdf（简单）看PDF，

视频播放其VLC

音乐播放器Exaile

文本编辑器VIM、gedit

浏览器Chromium

输入法Fcitx小企鹅+sunpinyin

音量托盘图标volumeicon

## 安装

首先需要安装X11，`#pacman -S xorg-server xorg-xinit`，需要界面的东西离不开这两个东西

安装yaourt，添加源`#vim /etc/pacman.conf`

    [archlinuxfr]
    SigLevel=Never
    Server=http://repo.archlinux.fr/$arch

更新`#pacman -Syy`，然后就可以安装了`#pacman -S yaourt`

- Tips：可以先安装bash-completion，`#pacman -S bash-completion`

安装窗口管理器和配置工具，`#pacman -S openbox obmenu obconf menumaker`，然后编辑`.initrc`文件，在最后加上`exec openbox-session`，将`/etc/xdg/openbox/`中的文件复制到`$HOME/.config/openbox/`下

安装DM，`#pacman -S slim`，然后添加开机启动`#systemctl enable slim@service`

安装面板以及tint2配置工具tintwizard，`yaourt -S tint2-svn tintwizard`，tintwizard并不支持快捷启动栏，而且会自动删除不支持的配置，所以每次配置后需要手动修改`$HOME/.config/tint2/tint2rc`，所以这个配置工具我用了一次就卸了，然后手动添加快捷启动栏的快捷方式，最后将`tint2 &`加入Openbox的启动脚本`$HOME/.config/openbox/autostart`中

安装系统监测conky，`yaourt -S conky-lua`，相对于conky多了对lua的支持，不需要另外配置lua，同样将`conky &`添加到Openbox启动脚本中

安装文件管理器，`#pacman -S pcmanfm ntfs-3g`，安装完后如果需要显示桌面，可以将`pcmanfm --desktop &`替代`conky &`

安装编辑器，`#pacman -S vim gedit`

安装音量图标，`#pacman -S volumeicon`，将`volumeicon &`加到`tint2 &`后面

安装输入法，`#pacman -S fcitx-im sunpinyin`，编辑`.xinitrc`，加入环境变量

```bash
    export GTK_IM_MODULE=fcitx
    export QT_IM_MODULE=fcitx
    export XMODIFIERS="@im=fcitx"
```

添加`fcitx &`到Openbox启动脚本，在`tint2 &`后

办公软件，`#pacman -S libreoffice mupdf`
视频音乐，`#pacman -S vlc`，`yaourt -S exaile`
浏览器，`#pacman -S chromium`

顺便添加张桌面壁纸，`#pacman -S feh`，在Openbox启动脚本中加上`feh  --bg-scale '/path/of/desktop_image.jpg &`

> 基本环境已经完成 虽然丑 至少能用
