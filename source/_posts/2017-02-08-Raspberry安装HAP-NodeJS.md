---
layout: post
title: 'Raspberry安装HAP-NodeJS'
date: 2017-02-08 00:27:40
comments: true
tags:
  - Raspberry Pi
  - HomeKit
  - HAP-NodeJS
---

# 下载 Raspbian

https://www.raspberrypi.org/downloads/raspbian/

# 制作 TF card

```
df -h
diskutil unmount /dev/disk2s1
diskutil list
dd bs=4m if=2017-01-11-raspbian-jessie.img of=/dev/rdisk2
diskutil unmountdisk /dev/disk2
```

<!--more-->

> note：新的img默认没有打开ssh，在tf卡根目录新建名称为 ssh 的文件夹解决

# 登录树莓派

```
ssh pi@raspberrypi.local
```

# 更新源

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo vi /etc/apt/sources.list
```

进入编辑界面，用 **#** 注释掉原来的源，添加下方的源。

```
deb http://mirrors.neusoft.edu.cn/raspbian/raspbian jessie main contrib non-free rpi
```

# 更新

```
sudo apt-get update && sudo apt-get upgrade -y
```

# 安装解决依赖

```
sudo apt-get install avahi-daemon avahi-discover libnss-mdns libavahi-compat-libdnssd-dev  build-essential -y
service avahi-daemon start
```

# 增加nodejs源

```
cd ~
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
```

# 安装nodejs

```
cd ~/Downloads
wget http://nodejs.org/dist/v7.6.0/node-v7.6.0.tar.gz
tar -xzf node-v7.6.0.tar.gz
cd node-v7.6.0
./configure
make
sudo make install
```

# 克隆HAP-NodeJS源码

```
git clone https://github.com/KhaosT/HAP-NodeJS.git
```

# 安装node-gyp

```
sudo npm install -g node-gyp
```

# 切换到HAP-NodeJS文件夹下，运行

```
sudo npm istall
node Core.js
```
