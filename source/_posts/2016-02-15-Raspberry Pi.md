---
layout: post
title: 'Raspberry Pi'
date: 2016-02-15 21:34:00
comments: true
tags:
  - 创客
  - Raspberry Pi
---

# Mac OS X下给树莓派安装Raspbian系统

先到[树莓派的官方网站](http://www.raspberrypi.org/downloads)去下载一个系统镜像，推荐下载[Raspbian](http://downloads.raspberrypi.org/images/raspbian/2013-02-09-wheezy-raspbian/2013-02-09-wheezy-raspbian.zip)。

如果用了直接扩展的WaveShare的TFT小屏，到 http://www.waveshare.net/wiki/RPI_IMAGE 去下载。

<!--more-->

下载完成后得到一个img镜像：

```bash
ls -lh
total 3788800
-rw-r--r--  1 mymacbook  staff   1.8G  2  9 03:44 2015-11-21-raspbian-jessie.img
```

插入SD卡，用df命令查看当前已挂载的卷：

```bash
df -h
Filesystem      Size   Used  Avail Capacity  Mounted on
/dev/disk0s2   112Gi   96Gi   15Gi    87%    /
devfs          183Ki  183Ki    0Bi   100%    /dev
map -hosts       0Bi    0Bi    0Bi   100%    /net
map auto_home    0Bi    0Bi    0Bi   100%    /home
/dev/disk2s1    15Gi  2.3Mi   15Gi     1%    /Volumes/disk2s1
```

对比Size和Name可以找到SD卡的分区在系统里对应的设备文件（这里是/dev/disk2s1），如果你有多个分区，可能还会有disk1s3之类的。使用diskutil unmount将这些分区卸载：

```bash
diskutil unmount /dev/disk2s1
2
Volume 未命名 on disk2s1 unmounted
```

通过diskutil list来确认设备：

```bash
diskutil list
/dev/disk0
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *121.3 GB   disk0
   1:                        EFI                         209.7 MB   disk0s1
   2:                  Apple_HFS Macintosh HD            120.5 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
/dev/disk2
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *15.8 GB    disk2
   1:               Windows_NTFS 未命名                  15.8 GB    disk2s1
```

使用 **dd** 命令将系统镜像写入，需要特别特别注意disk后的数字，不能搞错！
（说明：/dev/disk2s1是分区，/dev/disk2是块设备，/dev/rdisk2是原始字符设备）

```bash
dd bs=4m if=2013-02-09-wheezy-raspbian.img of=/dev/rdisk2
```

经过几分钟的等待，出现下面的提示，说明SD卡刷好了：

```bash
462+1 records in
462+1 records out
1939865600 bytes transferred in 163.133220 secs (11891297 bytes/sec)
```

用diskutil unmountDisk卸载设备：

```bash
diskutil unmountdisk /dev/disk2
Unmount of all volumes on disk2 was successful
```

现在就可以拔下SD卡，插到树莓派上启动系统了。

# 用户名&密码

默认用户是 **pi** ，密码是 **raspberry**

# 使用raspi-config配置树莓派

```bash
sudo raspi-config
```

http://shumeipai.nxez.com/2013/09/07/raspi-config-configuration-raspberry-pie.html

# Raspberry Pi(树莓派)国内软件源：

http://www.linuxidc.com/Linux/2013-10/91012.htm

修改配置文件

```bash
vi /etc/apt/sources.list
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ wheezy main contrib non-free rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ wheezy main contrib non-free rpi
deb http://mirrors.neusoft.edu.cn/raspbian/raspbian/ wheezy main contrib non-free rpi
deb-src http://mirrors.neusoft.edu.cn/raspbian/raspbian/ wheezy main contrib non-free rpi
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ wheezy main contrib non-free rpi
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ wheezy main contrib non-free rpi
```

首先我运行下列指令，确定我用的是最新系统软件。

```bash
sudo apt-get update && apt-get upgrade -y
```

配置树莓派支持中文

```bash
sudo apt-get install ttf-wqy-zenhei
sudo apt-get install scim-pinyin
```

# 为树莓派做系统备份镜像（for Linux & Mac）

http://shumeipai.nxez.com/2014/06/01/do-system-backup-image-of-raspberry-pi-for-linux-or-mac.html

# 魔镜

1. 信息模式（kioskmode）

现在就是要确定树莓派能够显示我在信息模式（kioskmode）下用Chromium浏览器显示的网页。Chromium浏览器是个能在树莓派操作系统上运行的开源浏览器。

一如既往，安装很简单：

```bash
sudo apt-get install chromium x11-xserver-utils unclutter
I have a Raspberry Pi 2 running Raspbian Jessie with Chromium 45 installed. I found that you need to copy the existing autostart file to your local:
cp /etc/xdg/lxsession/LXDE-pi/autostart /home/pi/.config/lxsession/LXDE-pi/autostart
So edit this local version:
sudo nano /home/pi/.config/lxsession/LXDE-pi/autostart
#@xscreensaver -no-splash  # comment this line out to disable screensaver
@xset s off
@xset -dpms
@xset s noblank
@chromium --incognito --kiosk http://localhost/  # load chromium after boot and point to the localhost webserver in full screen mode
Save
sudo reboot
```

这样就能完全禁用所有屏保功能，及Chromium浏览器在开机后自动启动，开启全屏模式并导向本地主页。

2. 设置网页

现在我们有了个多功能 Kiosk 啦。就缺个真实的网页了。我自己写了个应用，你可以随便修改或者直接用它。
我决定使用 PHP 的微架构 Lumen 以及一些 jQuery 和 MomentJs。

安装网页服务

要操作网页你首先得安装网页服务，要跑 Lumen 你首先得安装 php ：

```bash
sudo apt-get install nginx php5-fpm php5-cli php5-mcrypt git
```

这需要一点时间，不过你需要知道 MirrorMirror 网页安装在哪里，以便配置 nginx ：

```bash
sudo nano /etc/nginx/sites-available/mirror.conf
```

把以上配置复制到配置文件中：

```bash
server {
  listen 80;
  server_name localhost;
  root /home/pi/projects/MirrorMirror/public;
  index index.html index.htm index.php;
  charset utf-8;
  location / { try_files $uri $uri/ /index.php?$query_string; }
  location = /favicon.ico { access_log off; log_not_found off; }
  location = /robots.txt { access_log off; log_not_found off; }
  access_log off;
  error_log /var/log/nginx/myapp-error.log error;
  sendfile off;
  client_max_body_size 100m;
  location ~ .php$ {
    fastcgi_split_path_info ^(.+.php)(/.+)$;
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_index index.php; include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_intercept_errors off;
    fastcgi_buffer_size 16k;
    fastcgi_buffers 4 16k;
  }
  location ~ /.ht { deny all; }
}
```

然后激活这些配置：

```bash
sudo ln -s /etc/nginx/sites-available/mirror.conf /etc/nginx/sites-enabled/mirror.conf
sudo unlink /etc/nginx/sites-enabled/default
sudo service nginx reload
```

现在你的派里有了适用php的网页服务了！

3. 安装 MirrorMirror

这是最后一步了，安装真实的网页。在 /home/pi 里建一个文件夹并进入：

```bash
mkdir /home/pi/projects
cd /home/pi/projects
```

用下面的命令克隆 MirrorMirror 网页应用：

```bash
git clone https://github.com/ctrlaltdylan/MirrorMirror.git
```

安装配置器以便安装 Lumen 的相关依赖：

```bash
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

使用配置器安装 Lumen 的依赖，启动 MirrorMirror 并运行：

```bash
cd MirrorMirror
composer install
sudo chmod -R 777 storage
```

进入 http://localhost 就可以看到你的 MirrorMirror 的版本以及运行结果啦！

目前的功能：

- 显示天气预报以及萌萌的气象图案。
- 显示日期和时间。
- 显示一句随机的问候语，目前只有一句，不过计划会加更多啦。
  当然为了更简单的安装和定制化还需要更多的工作，不过我已经在思考怎么改善它了。希望你像我一样享受建造和使用你自己的魔镜。

树莓派的BIOS设置储存在系统启动分区中。在这分区中，有一个config.txt文件，载有所有设置。要旋转显示器，在这文件内加上以下一行代码：

```bash
display_rotate=1
```

要让显示器连接更加可靠，我就不解释为何加上下面允许HDMI线热插拔的代码了：

```bash
hdmi_force_hotplug=1
```

Other:

```bash
sudo apt-get update && apt-get upgrade -y
sudo apt-get install apache2 apache2-doc apache2-utils
sudo apt-get install libapache2-mod-php5 php5 php-pear php5-xcache
sudo git clone https://github.com/jangellx/MagicMirror.git
```

树莓派磨制“魔镜”全记录
http://askubuntu.com/questions/19898/whats-the-simplest-way-to-edit-and-add-files-to-var-www
你今天太帅了：来用树莓派打造一个魔镜吧
http://blog.dylanjpierce.com/raspberrypi/magicmirror/tutorial/2015/12/27/build-a-magic-mirror.html
