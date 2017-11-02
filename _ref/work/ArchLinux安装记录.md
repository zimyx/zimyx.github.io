---
title:  "Arch Linux 安装记录"
---


2017-06-15
[TOC]
{% include toc %}

## 安装准备

### 硬件型号

Dell OptiPlex 7050 Micro  

CPU  i7-7700T @ 2.90GHz   ，RAM 16.0GB

显示器 Dell E2216H  ，显卡 intel HD Graphics 630 (1920*1080 ？5912) , 声卡 Realtek Audio

硬盘：HGST HTS721010A9E630  1TB  、固态 LITEON CV3-8D128-11 SATA 128GB    

网络: 蓝牙, Intel(R) Dual Band Wireless-AC 8265, Intel(R) Ethernet Connection (5) I219-LM

操作系统 64位 WIN10 家庭中文版  产品ID：00342-34700-05600-AAOEM

### 安装介质

下载镜像文件[archlinux-2017.06.01-x86_64.iso](http://mirrors.163.com/archlinux/iso/latest/)安装包。

<u>windows 下</u>：  用USB格式化工具**Rufus **刻录到U盘。

<u>linux下</u>    ： 先umount U盘       $ sudo  dd bs=4M if=/home/zimy/Desktop/archlinux-2017.08.01-x86_64.iso of=/dev/sd？  status=progress && sync

**UEFI模式测试**

  如果你想使用UEFI，并且你的机器支持UEFI，你在引导时应该会看到如下选项：

​      ArchLinux archiso x86_64 UEFI USB

​      UEFIShell x86_64 v1

​      UEFIShell x86_64 v2

​      EFIDefault Loader

你应该可以知道Archlinux只提供64位的UEFI模式安装，32位无法引导。如果你忘记了是否出现过上述选项，你也可以使用命令：#efivar–l检查，如果你看到了一大串内容，那就是进入了UEFI安装模式。否则，你的电脑可能不支持UEFI或者你没进入UEFI安装模式，在后续内容中，你应该选择BIOS和MBR。

### 测试网络

#### 使用网线固定IP

设置静态ip  ： ArchLinux 默认开启DHCP。首先关闭DHCP：#systemctlstop dhcpcd.service，找出网卡名字#ip link（本次为enp3s0），临时设置网卡方法（**重启后失效**）

```shell
# ip link set enp3s0 up//激活网卡
# ip addr add 113.57.196.37/29 dev enp3s0 //设置ip地址和掩码
# ip route add default via 113.57.196.33//添加路由
最后更改DNS服务器地址
#nano /etc/resolv.conf
  nameserver 218.104.111.122
  nameserver dns_ip2
```

完成配置pingwww.baidu.com验证。否则可重启网络/etc/rc.d/networkrestart 或 修改/etc/rc.conf  

#### 使用无线网络

则可用wifi-menu命令配置。

```sh
# wifi-menu-o                        //在目录 /etc/netctl/ 中生成一个配置文件，例如wlp3s0-zbcc-wan。
#cat /etc/netctl/examples/wireless-wpa-static  >>/etc/netctl/wlp3s0-zbcc-wan  
//将例子追加到配文件尾，并修改IP为192.71.9.93/24,网关、DNS等(carbon x1)
#netctl start  wlp3s0-zbcc-wan                                         //运行配置文件。 
#netctl  enable  wlp3s0-zbcc-wan                                       //建立开机自运行服务
```

### 磁盘分区

#### 创建分区 

 一般先查一下当前分区情况#lsblk，当前普通硬盘 sda固态硬盘sdb， 下面是拟定方案： 

> /dev/sdb1  500M    /boot   物理分区 
> /dev/sdb2  18G       swap   物理分区
> /dev/sdb3  100.8G   /          物理分区
> /dev/sda1  931.5G   /          物理分区

硬盘测速命令： hdparm -tT /dev/sdb
*MBR分区可以使用工具fdisk。 #fdisk /dev/sda*
*分区的方法都比较相似，使用m，可以列出所有命令，请根据提示分区。*

如果是GPT分区，可以使用的工具是gdisk。（或者工具parted）

```shell
#gdisk /dev/sdb
   Command (? for help): o 						创建新的空GUID分区表
   This option deletes all partitions and creates a new protective MBR.
   Proceed? (Y/N): Y
   Command (? for help): p 显示分区信息
   Disk /dev/sdb: 234441648 sectors, ..GiB  ...
   Number  Start (sector)    End (sector)  Size       Code  Name
新建第一个分区： 系统引导分区500M
   Command (? for help): n  						
   Partition number (1-128, default 1):
   First sector (34-234441614, default = 34) or {+-}size{KMGTP}:
   Last sector (2048-234441614, default = 234441614) or {+-}size{KMGTP}: +500M
   Current type is 'Linux filesystem'
   Hex code or GUID (L to show codes, Enter = 8300): ef00
   Changed type of partition to 'EFI System'
新建第二个分区： swap交换分区18G
   Command (? for help): n  						
   Partition number (2-128, default 2):
   First sector (34-234441614, default = 105269248) or {+-}size{KMGTP}:
   Last sector (105269248-234441614, default = 234441614) or {+-}size{KMGTP}: +18G
   Current type is 'Linux filesystem'        
   Hex code or GUID (L to show codes, Enter = 8300): 8200
   Changed type of partition to 'Linux swap'
新建第三个分区： LINUX的根分区   
   Command (? for help): n  
   Partition number (3-128, default 3):
   First sector (34-234441614, default = 105269248) or {+-}size{KMGTP}:
   Last sector (105269248-234441614, default = 234441614) or {+-}size{KMGTP}:
   Current type is 'Linux filesystem'        
   Hex code or GUID (L to show codes, Enter = 8300): 8304
 最后显示分区后信息检查&保存
   Command (? for help): p
   Command (? for help): w写保存退出   
     Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
     PARTITIONS!!

     Do you want to proceed, possibly destroying your data? (Y/N): Y
     OK; writing new GUID partition table (GPT).
     The operation has completed successfully.  
另一块盘 sda： o,n, , , ,8300,w,y
```

#### 建立文件系统（格式化分区）

这里我们使用的FS是比较成熟的ext4，至于最新的btrfs，“据说”btrfs对SSD有优化，大家可以尝试下。
```
#mkfs.ext4     /dev/sdb3          格式化 /
#mkswap        /dev/sdb2         格式化交换区
#swapon        /dev/sdb2          开启交换分区
对于/boot这个分区----使用MBR的命令如下：#mkfs.ext4 /dev/sdb1
而GPT分区，使用命令：#mkfs.vfat /dev/sdb1
#mkfs.ext4    /dev/sda1           格式化另块1T盘
```
#### 挂载分区

```
#mount /dev/sdb3 /mnt
#mkdir /mnt/{boot,home}
#mount /dev/sdb1 /mnt/boot
#mkdir /mnt/Tdisk
#mount/dev/sda1 /mnt/Tdisk        //挂载另块1T盘到  Tdisk
```

### 更新安装源

**#sed-i "s/^\b/#/g" /etc/pacman.d/mirrorlist**修改mirrorlist文件每行加注释#
**#nano/etc/pacman.d/mirrorlist **推荐使用ustc或者163的源去掉对应地址前#
**#pacman–Syy   ** 强制同步更新全部软件包列表

另外可用rankmirrors命令的方法将镜像列表按速度排列
\#cd /etc/pacman.d                                            cd到/etc/pacman.d/目录:
\#cp  mirrorlist  mirrorlist.backup			备份
\#rankmirrors  -n  6  mirrorlist.backup > mirrorlist                 -n6:将生成6个最接近的源



## 安装配置基本系统

### 下载安装基本系统

`# pacstrap -i  /mnt base  base-devel`
### 生成fstab文件

`#genfstab -U -p /mnt >> /mnt/etc/fstab`
用 -U 或 -L 选项设置UUID 或卷标，记录了挂载的相关信息，Archlinux中提供了工具来一键生成这里使用的是UUID，如果不加-U，那么在fstab中记录的就是/dev/sdX之类的地址了（这样设备插拔后顺序会乱） 

### Chroot、主机名、时区、本地化

```shell
切换根目录
# arch-chroot /mnt                  Change root 到新安装的系统
# alias ls='ls –color'               ls显示颜色，方便查看。
设置主机名
# echo computer_name>/etc/hostname         如 zbcc_dell
# nano /etc/hosts     修改/etc/hosts文件的内容,把该文件中的“主机名”替换成zbcc_dell
    127.0.0.1   localhost.localdomain localhost  主机名
    ::1     localhost.localdomain localhost  主机名
设置时区
# ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime   (查看时区用tzselect)
# hwclock --systohc --utc                     设置时间标准 为 UTC，并调整 时间漂移
 //  #date –s   hh:mm:ss     修改时间
 //  #hwclock  -w		     将当前时间写入CMOS中
设置Locale本地化
#nano /etc/locale.gen        开启en_US.UTF-8;zh_CN.UTF-8;zh_CN.GB18030;zh_CN.GB2312
#locale-gen                                 生成指定的本地化文件
# echo LANG=en_US.UTF-8 > /etc/locale.conf  创建 locale.conf 并提交您的本地化选项
```
### 创建用户

```shell
# passwd                                               设置root的密码
# useradd -m -g users -G wheel -s /bin/bash zimy     用户名创建用户zimy
# passwd zimy                               这里添加wheel用户组是为该用户能够使用sudo提权
```
### 生成初始ramdisk环境

`# mkinitcpio	-p	linux ` 	（在 pacstrap 安装[linux](https://www.archlinux.org/packages/?name=linux) 时就会自动运行 [mkinitcpio](https://wiki.archlinux.org/index.php/Mkinitcpio)，大部分用户都可以使用 mkinitcpio.conf 默认设置，如果有定制需求才使用）
初始 RAM磁盘（initrd）是在系统引导过程中挂载的一个临时根文件系统，用来支持两阶段的引导过程。一般用于无盘环境或通过initrd自定义内核加载模块。硬件变更时需要重新运行该命令。

### 安装引导程序

对于使用UEFI的用户，这里使用自带的systemd-boot。
```shell
# bootctl install
bootctl 要创建 /boot/loader/entries/arch.conf 并添加以下内容，别忘了把 /dev/sdaX 改为您的实际根分区
# nano /boot/loader/entries/arch.conf
   title          Arch Linux
   linux          /vmlinuz-linux
?  initrd         /intel-ucode.img   (启用Intel cpu微指令更新，Intel CPU 需要安装 intel-ucode 并根据 Microcode 配置 boot loader.) 
   initrd         /initramfs-linux.img
   options           root=/dev/sdaX  rw         重要：该处实际根分区应考虑重启后设备号变化，本次为sda2
更安全的做法可以用#blkid查询root分区的PARTUUID，例如          详见：Systemd-boot
options       root=PARTUUID= 22ffcc5d-2708-46ee-997d-c1812ed3106e rw

可选修改 /boot/loader/loader.conf文件
```
对于BIOS用户，推荐使用GRUB。
\#pacman-S grub-bios 
\#grub-install/dev/sdX 
\#grub-mkconfig-o /boot/grub/grub.cfg
这三条命令分别是使用pacman获取grub，将引导信息写到sdX，以及生成配置文件grub.cfg。

### 重启

如果是无线网络：
要在重启前安装 iw, dialog 和 wpa_supplicant, 您要靠它们连网：# pacman –S  iw  wpa_supplicant  dialog

```shell
需要退出chroot，卸载分区，然后直接reboot，移除安装介质。
# exit
# umount /mnt/boot  /mnt/Tdisk
# umount /mnt
# reboot
```



## 桌面环境安装

### 配置网络

#### 静态有线 IP设置

```shell
警告: 请使用#systemctl--type=service确保其它配置网络的服务都没有运行，同时使用多个网络配置工具会冲突。
#nano  /etc/systemd/network/wired.network
 [Match]
  Name=enp3s0
 [Network]
  Address=113.57.196.37/29
  Gateway=113.57.196.33

#nano /etc/resolv.conf   增加DNS： nameserver 202.103.24.68
#systemctl enable systemd-networkd.service      起服务
#reboot
```

相关网络命令： ip link 查看网络接口，ip address 显示ip地址(可选 -s 参数statistics，如ip -s addr)，ip neigh 邻居表

#### 无线

配置参考前面:[使用无线网络](#1.1.3.2)

### 安装图形环境

用户图形化交互的标准命名为“X Window System"，通常简称为*X11*或是*X* ，Xorg-X11使用X11的接口和标准,Xorg是在想运行的硬件和图形软件之间提供了一个接口。

```shell
#pacman-S  xorg-server  xorg-apps
安装显卡驱动
#lspci| grep VGA               检查显卡型号
#pacman –S  xf86-video-intel 
#pacman -S libva-intel-driver   使用新GPU 的硬件编解码功能，选装
```

说明：显卡是AMD/ATI的改为xf86-video-ati；是GeForce7的改为nvidia；想装通用的显卡驱动，把命令中的则用xf86-video-vesa 。或者用命令**#pacman-Ss xf86-video | less **查看匹配

## 桌面基础应用

```shell
#timedatectl set-ntp true   启用时间同步
#timedatectl                                    检查时间同步状态
#systemctl status systemd-timesyncd               时间同步
---- 由此处起 可执行自制脚本批量安装，如下  
  curl -O  https://raw.githubusercontent.com/zimyx/tool/master/ArchlinuxSH
建议提前安装原生jre，否则会自动安装openjdk。安装yaourt后，装jdk8（jdk9目前对xmind、dbeaver依赖eclipse支持不好）; 用archlinux-java命令可切换、设置缺省java环境。
#pacman -S adobe-source-han-serif-cn-fonts adobe-source-han-sans-cn-fonts wqy-microhei                    思源宋体、黑体,wqy-microhei必装（有些程序依赖通用TTF字体）
#pacman -S noto-fonts-emoji                      Google的emoji 字体
#pacman  -S lightdm lightdm-gtk-greeter      安装LightDM登录管理器  (显示管理器)或用LXDM   
#pacman –S  xfce4   xfce4-goodies            安装xfce4桌面 输入startxfce4进入 (桌面环境) 
#systemctl start lightdm.service
#systemctl enable lightdm.service
安装基础工具软件
#pacman –S  gvfs                                 自动挂载usb设备
#pacman –S  slock                                小巧轻便的锁屏工具
#pacman -S  bash-completion                      自动命令补全
#pacman -S  chromium min                     安装google和min浏览器
#pacman -S  libreoffice-fresh-zh-CN              安装OFFICE
#pacman -S  unrar unzip p7zip  (AUR)libnatspec   支持解压zip、rar、7z和支持字符转换的工具libnatspec
#pacman -S  file-roller                          图形化解压工具
#pacman -S  evince   mypaint                     PDf / DjVu 等文档阅读工具，和绘图工具
#pacman -S  catfish                              文件搜索功能
#pacman -S  wget                      必备下载工具，特别适合下载zip文件(使用wget -c断点续传)
#pacman -S  remmina freerdp gnome-keyring        远程桌面(freerdp是支持win插件，gnome-keyring支持密码保存)
#pacman -S  fcitx fcitx-im fcitx-sogoupinyin fcitx-configtool                       输入法（如无法配置输入法,需要安装fcitx-configtool并运行该工具配置） 
非桌面环境在 ~/.xprofile 中加入以下代码
  export GTK_IM_MODULE=fcitx
  export QT_IM_MODULE=fcitx
  export XMODIFIERS=@im=fcitx 
# sudo pacman -S alsa-utils  vlc  mpv          声卡驱动(alsamixer调整声道音量)，媒体播放器
#sudo chown zimy Tdisk                           变更Tdisk盘的文件夹用户所有权
//自动被依赖 #pacman -S  openssh                  远程登录命令ssh -p 端口 root@IP Addr
#pacman -S  thunderbird                     邮件客户端 ( 启动方式 env LANG=zh_CN.UTF-8 )
邮件迁移方法 拷贝原文件夹 ~/.thunderbird/?????.default/  到新装机相同路径下 
执行命令 thunderbird -ProfileManager 创建新配置，删除不要的
#pacman -S  gvfs-smb sshfs       //安装两个包用于通过 Thunar 文件管理器浏览远程sabam共享目录
```

## 中文化,XX,打印

**配置sudo：**

为了系统管理的方便，配置sudo是很有必要的：先安装   ``` pacman -S sudo```
在上一步中，已经将用户加入了wheel组，当用户使用sudo命令时，要使系统给予wheel组中所有用户完全的root权限，只需要以root身份执行visudo命令，编辑sudo的配置文件，将下面一行取消注释：
%wheel    ALL=(ALL) ALL
这样，当用户使用sudo命令时，就可以以root身份执行命令了。

### 系统中文化

```shell
#sudo nano ~/.xprofile            增加如下配置
  export LANG=zh_CN.UTF-8                            
  export LANGUAGE=zh_CN:en_US
  export LC_CTYPE=en_US.UTF-8
```

**应用中文化：** `env LANG=zh_CN.UTF-8  + 应用程序路径`
libreoffice、chromium  中文化在应用中的菜单设定

### 科学NET

* GitHub上下载XX-Net-n.n

* 安装 `sudo pacman -S python2-notify  python2-pyopenssl `

* 运行`sudo ./start ` 打开页面127.0.0.1:8085  检查

* 安装SwitchyOmega.crx，及导入OmegaOptions.bak

* 有可能导入CA认证不成功，可在Chromium设置-高级-管理证书界面手工导入XX-Net文件夹 data\gae_proxy 目录下的 "CA.crt" 证书。

* 建立桌面快捷：../XX-Net/start (不勾选在终端运行)，如果启动不正常需检查文件夹的所有权并修改用chown。

* 开启IP V6 代理地址搜索

  ```sh
  # pacman -S miredo      //安装隧道软件
  # sudo systemctl start  miredo.service    //起服务生成虚拟网卡，可能要重启系统
  # sudo systemctl enable miredo.service 
  # nano  resolv.conf     //添加ip v6 的DNS 
  nameserver 202.103.24.68
  nameserver 218.104.111.122
  nameserver 2001:470:20::2
  ```

  IPv6 连接测试网站  http://test-ipv6.com/


### 打印

`#sudo pacman -S cups ghostscript gsfonts hplip  gtk3-print-backends  `         最后一个用来支持GTK3应用程序打印
安装完后需要启动服务才能配置打印机（HP Officejet Pro 8600）
systemctl start org.cups.cupsd.service
systemctl enable org.cups.cupsd.service
进入页面 http://localhost:631配置添加打印机，办公室打印机连接方式选socket://192.71.9.7:9100（用户名 密码：用本机root帐号）

**注意**：如果您可以从CUPS Web界面打印测试页，但不能从LibreOffice打印测试页，请尝试安装a2ps软件包。(并在cups管理界面设置默认打印机)

## 应用安装

### 安装 yaourt 和git

（方便AUR安装的程序升级）

```shell
git clone https://aur.archlinux.org/package-query.git
cd package-query
makepkg -si
cd ..
git clone https://aur.archlinux.org/yaourt.git
cd yaourt
makepkg -si
cd ..

新增AUR安装源,编辑/etc/pacman.conf文件末尾添加以下两行：
    用清华大学ArchlinuxCN镜像
      [archlinuxcn]
      Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
    或者用下面的 中国Yaourt镜像软件库
      [archlinuxcn]
      SigLevel = Optional TrustedOnly
      Server   =  http://repo.archlinuxcn.org/$arch
添加源之后,先用yaourt -Syu 同步一下再安装钥匙环 sudo pacman -S archlinuxcn-keyring 包导入 GPG key。
```
yaourt -S package_name – 从AUR安装软件包
yaourt -Ss password – 使用关键字搜索软件包
yaourt -Syu –aur – 从AUR升级本地软件数据库并安装更新
yaourt -Si package_name – 列出软件包信息
yaourt -Sc – 从缓存中清楚旧的软件包(包括卸载了的)
yaourt -Su – 安装AUR中的更新软件包
yaourt -Sy – 获取最新的AUR软件包数据库
yaourt -Cd – 清除AUR软件包数据库
yaourt -R package_name – 删除软件包

* 安装图形化Git同步工具 SmartGit ： yaourt -S  smartgit

* 安装dbeaver数据库工具，在AUR下载dbeaver-ee源

* 安装Oracle  jdk 版本8u131： yaourt -S  jdk   并按照提示设置该版本为缺省值

* 美化桌面  yaourt -S numix-icon-theme-git  numix-gtk-theme 。并在设置->外观和设置->窗口管理器中配置

* 安装makedown文本编辑器typora 
  1. 并导入自定义主题~/.config/Typora/themes/base.user.css  (从github上载入)
  2. 快捷键调整： 编辑文件~/.config/Typora/conf/conf.user.json中增加"Source Code Mode": "Ctrl+."

* 安装AsciiDoc 编辑器 Gitbook

  yaourt  -S  gitbook-edit         参考 [中文说明](https://chrisniael.gitbooks.io/gitbook-documentation/format/plugins.html)  和   [插件站点](https://plugins.gitbook.com/plugin/)

### 安装私有云

  1. yaourt -S rslsync

  2. 在/Tdisk/Zcloud目录下执行rslsync --dump-sample-config > sync.conf 生成配置文件

  3. 执行命令并打开网页配置   rslsync --webui.listen 0.0.0.0:8888

  4. 创建上面命令的桌面快捷方式，注意防火墙 

  5. 修改了/etc/hosts    ？？https://www.zhihu.com/question/60919926

    参考 http://www.cnblogs.com/zhanmeiliang/p/5922946.html

### 安装图形工具

#### yEd Graph Editor 

安装 ：yaourt -S  yed   (前提：安装Oracle jdk 不能用openJDK )

为yed文件添加自定义**MIME类型关联**

  1. 新建 /usr/share/mime/packages/x-yed.xml文件：

     ```
     <?xml version="1.0" encoding="utf-8"?>
     <mime-info xmlns="http://www.freedesktop.org/standards/shared-mime-info"> 

         <mime-type type="image/yed+xml">
             <comment>yEd image</comment>
             <comment xml:lang="en_GB">yEd image</comment>
             <comment xml:lang="zh_CN">yEd 图像</comment>
             <sub-class-of type="application/xml"/>
             <glob pattern="*.graphml"/>
         </mime-type>
          
         <mime-type type="image/yed+xml-compressed">
             <comment>compressed yEd image</comment>
             <comment xml:lang="en_GB">compressed yEd image</comment>
             <comment xml:lang="zh_CN">压缩的 yEd 图像</comment>
             <sub-class-of type="application/gzip"/>
             <glob pattern="*.graphmlz"/>
         </mime-type>

     </mime-info>
     ```

     上述 x-yed.xml文件定义了两种新的 MIME 类型“image/yed+xml”，并指定拓展名是.graphml 的文件为该 MIME 类型。

  2. 修改/usr/share/applications/yed.desktop文件：

     在最后增加一行：MimeType=image/yed+xml;image/yed+xml-compressed;

  3. 以 root 身份更新 MIME 数据库以使更改生效：

     ```
     # update-mime-database /usr/share/mime
     ```

  4. 以 root 身份更新应用程序数据库：

     ```
     # update-desktop-database /usr/share/applications
     ```

  5. 重启一下，应该OK并会默认关联一种图标，检查用：file --mime-type XX.graphml   命令

   [参考链接](https://access.redhat.com/documentation/zh-CN/Red_Hat_Enterprise_Linux/7/html/Desktop_Migration_and_Administration_Guide/File_Formats.html)Redhat中文说明

#### 思维导图工具

```
Yaourt -S  xmind
```

```
关联文件类型
<?xml version="1.0" encoding="utf-8"?>
<mime-info xmlns="http://www.freedesktop.org/standards/shared-mime-info"> 

    <mime-type type="application/xmind">
        <comment>XMind</comment>
        <comment xml:lang="en_GB">XMind</comment>
        <comment xml:lang="zh_CN">思维导图</comment>
        <glob pattern="*.xmind"/>
    </mime-type>
 
 </mime-info>
```

xmind 3.7.4-1版本的升级,参看AUR 该包介绍下的评论要: 

``` 
$ cp -r /usr/share/xmind/XMind/configuration  ${HOME}/.xmind/   和  rm -r ~/.xmind 才行
```



### 共享文件夹

示例为CentOS上的共享

1. 安装 samba

   ```sh
   # yum install samba
   # su zimy
   $ mkdir /home/zimy/shared
   $ sudo chmod -R 0777 shared/
   ```

2. 修改conf 配置文件

```sh
# vi /etc/samba/smb.conf
	[global]
	workgroup = WORKGROUP                               //修改
	hosts allow = 127. 192.71.9. 172.18.2. 202.100.168. //修改
	security = user                    //share最低，其次是user(密码),最高是server
	
	[myshare]                                           //新增
	comment=my shared files
	path=/home/zimy/shared
	public=yes
	writeable=yes
```

1. 创建Samba用户帐户

   Samba用户帐户必须是已有的Linux用户，用smbpasswd工具来创建

   ```sh
   # smbpasswd -a  zimy
   ```

2. 起服务

   ```sh
   # systemctl start smb.service     centos6.4版用 ( service smb  start )
   # systemctl enable smb.service                  
   # systemctl is-active smb                       ( chkconfig --level 35 smb on )
   ```

   查看当前smb状态命令   smbstatus 
   通过Thunar连接   `smb://202.100.168.229/myshare/`

## 其他
 ###  由于密钥丢失的更新失败 ：   

     sudo yaourt -Syu      报 archlinuxcn 源 密钥错误   

     错误：所需的密钥从密钥环中丢失 
   用`sudo pacman -S archlinux-keyring && sudo pacman -Syu` 解决

### 用yaourt安装IDEA 报错空间不够

 增加/tmp空间 $sudo mount -t tmpfs -o size=4g tmpfs /tmp  。装完可调回来

