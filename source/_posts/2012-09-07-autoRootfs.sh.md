---
layout: post
title: 'autoRootfs.sh'
date: 2012-09-07 20:50
comments: true
tags:
  - 嵌入式
---

autoRootfs.sh

<!--more-->

```bash
#!/bin/sh
echo "------autoRootfs-------"
# 设定目录
TARGET_DIR=/home/at91sam9/mdk9261/rootfs/jffs2
# =====================================
echo "create rootfs"
cd $TARGET_DIR
if [ ! -d root ]
then
mkdir root dev etc bin sbin mnt sys proc lib home tmp var usr
mkdir usr/sbin usr/bin usr/lib usr/modules
mkdir etc/init.d
mkdir lib/modules
chmod 777 tmp
fi
# =====================================
echo "create dev nod"
cd $TARGET_DIR/dev
###################
if [ ! -e console ]
then
mknod -m 600 mem		c 1 1
mknod -m 666 null		c 1 3
mknod -m 666 zero		c 1 5
mknod -m 644 random		c 1 8
mknod -m 600 tty0		c 4 0
mknod -m 600 tty1		c 4 1
mknod -m 600 ttyS0		c 4 64
mknod -m 600 ttyS1		c 4 65
mknod -m 600 tty		c 5 0
mknod -m 600 console	c 5 1
mknod -m 755 mmcblk0    b 179 0
mknod -m 755 mmcblk0p1  b 179 1
chmod 644 random
chmod 600 tty0 tty1 ttyS0
ln -s /proc/self/fd fd
ln -s fd/0 stdin
ln -s fd/0 stdout
ln -s fd/0 stderr
fi
# =====================================
# 建立配置文件
###################
echo "etc/profile"
cd $TARGET_DIR/etc
if [ ! -e profile ]
then
cat <<EOT > profile
#!/bin/sh
#/etc/profile:system-wide .profile file for the Bourne shell
echo "Processing /etc/profile ... "
# no-op
# Set search library path
echo "Set search library path in /etc/profile"
export LD_LIBRARY_PATH=/lib:/usr/lib
# host name
HOSTNAME=`/bin/hostname`
export HOSTNAME
# Set user path
echo "Set user path in /etc/profile"
PATH=/bin:/sbin:/usr/bin:/usr/sbin
export PATH
# alias
alias ll="ls -al"
# Prompt
USER="`id -un`"
LOGNAME=$USER
PS1='[\u@\h \W]\$'
echo "Welcome to Linux"
EOT
fi
###################
echo "etc/inittab"
cd $TARGET_DIR/etc
if [ ! -e inittab ]
then
cat <<EOT > inittab
# /etc/inittab
#
# Copyright (C) 2009 Edutech <cuiqingwei@gmail.com>
#
# Note: BusyBox init doesn't support runlevels.  The runlevels field is
# completely ignored by BusyBox init. If you want runlevels, use
# sysvinit.
#
# Format for each entry: <id>:<runlevels>:<action>:<process>
#
# id        == tty to run on, or empty for /dev/console
# runlevels == ignored
# action    == one of sysinit, respawn, askfirst, wait, and once
# process   == program to run
# now run any rc scripts
::sysinit:/etc/init.d/rcS
# 不需要设置登录验证
#console::respawn:-/bin/sh
# 需要设置登录验证 Put a getty on the serial port
ttyS0::askfirst:/sbin/getty -L ttyS0 115200 vt100
# Logging
#null::respawn:/sbin/syslogd -n -m 0
#null::respawn:/sbin/klogd -n
# ctrl+alt+del restart
::ctrlaltdel:/sbin/reboot
# restart init process
::restart:/sbin/init
# Stuff to do before rebooting
#null::shutdown:/usr/bin/killall klogd
#null::shutdown:/usr/bin/killall syslogd
null::shutdown:/bin/umount -a -r
null::shutdown:/sbin/swapoff -a
# end 2012-09-22
EOT
fi
###################
echo "etc/fstab"
cd $TARGET_DIR/etc
if [ ! -e fstab ]
then
cat <<EOT > fstab
# /etc/fstab: static file system information.
#
# <File system> 	<mount pt>		<type>	<options>		<dump>	<pass>
#rootfs			/             	auto	defaults 				1	1
porc    		/proc         	proc 	defaults              	0	0
#tmpfs       	/dev         	tmpfs 	size=512k,mode=0755		0	0
#devpts      	/dev/pts       	devpts	mode=0620,gid=5  		0	0
usbfs         	/proc/bus/usb 	usbfs 	defaults              	0	0
# Here, we use 1MB memory as tmpfs,and mount on /var/volatile. Can be used to
# hold log files (Lost when shut down, but can avoid write NAND
tmpfs         	/var/volatile 	tmpfs 	size=1M               	0	0
# uncomment this if your device has a SD/MMC/Transflash slot
#/dev/mmcblk0p1	/mnt/mmc		auto   	defaults,sync,noauto  	0	0
#/dev/mtdblock1	/mnt/mtd     	jffs2  	defaults,sync,noauto  	0	0
EOT
fi
###################
echo "etc/init.d/rcS"
cd $TARGET_DIR/etc/init.d
if [ ! -e rcS ]
then
cat <<EOT > rcS
#!/bin/sh
echo "********************************"
echo " Exec rcS  "
echo "********************************"
runlevel=S
export runlevel
# Host name
/bin/hostname mdk9261
# Mount /proc if not
[ -d "/proc/1" ] || mount /proc

echo "********* mount all ************"
/bin/mount -av
mount -t tmpfs 	mdev   /dev
mount -t sysfs 	sysfs  /sys
mkdir /dev/pts
mount -t devpts devpts /dev/pts
# Read the busybox docs: mdev.txt
echo "******** starting mdev *********"
echo "This may take some time ..."
#/bin/mount -t sysfs sysfs /sys
/bin/echo /sbin/mdev > /proc/sys/kernel/hotplug
mdev -s
# When mdev is mounted, /sys can be umounted
#/bin/umount /sys
# Start local services
/etc/init.d/rc.local
echo "********************************"
echo " Linux for mdk9261 "
echo "********************************"
# by: cuiqingwei 20120913"
EOT
fi
###################
echo "etc/init.d/rc.local"
cd $TARGET_DIR/etc/init.d
if [ ! -e rc.local ]
then
cat <<EOT > rc.local
#!/bin/sh
echo "***** start local services *****"
ifconfig lo up
#udhcpc -t 3 -i eth0
echo "******* starting telnetd *******"
telnetd
#ntpd -p 209.81.9.7
#echo "******** starting boa **********"
#mkdir /var/log/boa
#boa
#echo "******** starting samba ********"
#nmbd -D
#smbd -D
#echo "******** mount MMC/SD **********"
#if [ -d "/dev/mmcblk0p1" ]; then
    mount -t vfat /dev/mmcblk0p1 /mnt
#fi
# by: cuiqingwei 20120913"
EOT
fi
###################
echo "etc/mdev.conf"
cd $TARGET_DIR/etc
if [ ! -e mdev.conf ]
then
touch mdev.conf
fi
###################
echo "etc/group"
cd $TARGET_DIR/etc
if [ ! -e group ]
then
cat <<EOT > group
root:*:0:
nobody:*:65534:
EOT
fi
###################
echo "etc/passwd"
cd $TARGET_DIR/etc
if [ ! -e passwd ]
then
cat <<EOT > passwd
root::0:0:root:home/root:/bin/sh
nobody:*:65534:65534:nobody:/nonexitent:/bin/sh
EOT
fi
###################
echo "etc/shadow"
cd $TARGET_DIR/etc
if [ ! -e shadow ]
then
touch shadow
fi
# =====================================
echo "----------END----------"
echo "by: cuiqingwei 20121106"
```
