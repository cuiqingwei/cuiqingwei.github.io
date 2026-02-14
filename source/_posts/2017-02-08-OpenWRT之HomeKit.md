---
layout: post
title: 'OpenWRT之HomeKit'
date: 2017-02-08 16:20:56
comments: true
tags:
  - OpenWRT
  - HomeKit
---

# 1.制备openwrt系统镜像

预编译的openwrt是musl的toolchain，而homebridge需要glibc的toolchain所以需要自己编译系统。

建议安装完成后开启ssh，直接用putty和winscp来传送文件和操作虚拟机更方便。
以下所有命令都是通过putty登陆虚拟机或使用虚拟机的terminal来执行的。

```
sudo apt update
```

安装编译系统需要的软件包:

```
sudo apt update
```

下载openwrt最新代码，

```
git clone git://github.com/openwrt/openwrt.git
```

<!--more-->

将feeds.conf.default改名为feeds.conf

更新feeds部分的代码

```
./scripts/feeds update -a
./scripts/feeds install -a
```

> 使用feeds文件夹替换掉openwrt，原版代码不带avahi的dev库，需要自己添加支持；
> 另外自带nodejs的版本低于5.0启动homebridge的时候会有个warning，一并更换成最新版。

openwrt编译配置：

```
make menuconfig
```

需要的软件包：
Advanced configuration options (for developers) --->
Toolchain Options --->
C Library implementation (Use musl) --->
[X] Use (e)glibc
Base system --->
block-mount、ca-certificates
Development --->
<M> gcc
<M> make
Kernel modules --->  
 Filesystems --->
kmod-fs-exfat、kmod-fs-ext4、kmod-fs-ntfs、kmod-fs-vfat
USB Support --->
kmod-usb-storage-extras、kmod-usb2、kmod-usb3
Languages --->
Node.js --->
<M> node
Python --->
<M> python
Libraries --->
libavahi-compat-libdnssd-dev
Network --->
mosquitto
LuCi ---> 1. Collections --->
luci

保存后开始编译

```
make V=s
```

趁着时候没事干把准备好的U盘连入虚拟机，格式化成ext3/ext4格式，插到路由器的USB接口上。

刷入系统不再详述，不会搞的可以百度，这方面的教程遍地。
成功后进入路由器管理界面luci，system下将语言改为中文。设置路由器登陆密码，然后使用putty和winscp即可连接到路由器文件系统了。
将U盘查到路由器上等一会以后，从路由器管理界面找到如下位置，看到U盘的大小说明成功。

# 2. openwrt 挂载overlay到U盘

- 2.1. 添加必要软件

**block-mount**
**kmod-fs-ext4**
**kmod-usb-ohci**
**kmod-usb-storage**
**fdisk**
**cfdisk**
**e2fsprogs**

可以在 **menuconfig** 通过输入 **/** 查找这些组件。

- 2.2. 升级系统

**sysupgrade -v xxxx-sysupgrade.bin**

- 2.3. 分区操作

**cfdisk**

分三个区
sda1（512M）为系统盘，相当于Windows系统的C盘；
sda2（128M)为交换分区，用于内存交换；
sda3(剩余)为存储区，主要放置较大文件

- 2.4.格式化各分区

```
mkfs.ext4 /dev/sda1 　　#将第一个分区格式化为ext3格式
mkswap /dev/sda2    　　#将第二个分区格式化为swap交换分区
mkfs.ext4 /dev/sda3 　　#将第三个分区格式化为ext3格式
```

- 2.5.安装完成以后,挂载u盘.如下命令将默认的系统盘转移到u盘中.

```
mount /dev/sda1 /mnt
mkdir /tmp/root
mount -o bind / /tmp/root
cp /tmp/root/* /mnt -a
umount /tmp/root
```

- 2.6.修改 /etc/config/fstab 文件,让系统启动自动挂载,修改成如下

```
config mount
        option target   /overlay
        option device   /dev/sda1
        option fstype   ext4
        option options  rw,sync
        option enabled  1
        option enabled_fsck 0
```

- 2.7.执行如下命令让其生效

```
/etc/init.d/fstab enable
```

- 2.8.重启后，执行df -h后如果发现/overlay挂载在/tmp/overlay-disabled上的解决方案：

**rm /tmp/overlay-disabled/etc/extroot.md5sum**

或者删除/tmp/overlay-disabled目录下的所有系统文件，按照第2.5重新操作重启。
