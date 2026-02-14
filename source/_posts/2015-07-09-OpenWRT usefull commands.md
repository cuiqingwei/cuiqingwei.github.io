---
layout: post
title: 'OpenWRT usefull commands'
subtitle: 'OpenWRT有用的命令'
date: 2015-07-09 18:00:00
tags:
  - OpenWRT
  - 备忘
---

# 不得不说

在命令行，按两次**TAB**键，列出所有命令！

# 硬件信息

- CPU:

```bash
cat /proc/cpuinfo
```

- Memory:

```bash
free
cat /proc/meminfo
```

- Disk Space:

```bash
df -h
cat /proc/mtd
cat /proc/partitions
```

当然还可以通过**dmesg**配合grep查看！

<!--more-->

# 其他信息

- 内核信息

```bash
uname -a
```

- 内核版本

```bash
uname -r
```

- kernel信息

```bash
dmesg
```

- 系统日志

```bash
logread
```

- 进程列表

```bash
ps
```

- 获取运行时间，系统负载率

```bash
uptime
```

**11:45:37 up 3 days, 10:46, load average: 0.00, 0.01, 0.04**
1、11:45:37 // 系统当前时间
2、up 3 days, 10:46 // 主机已运行时间,时间越大，说明你的机器越稳定。
3、load average // 系统平均负载，统计最近1，5，15分钟的系统平均负载

- 查看环境变量

```bash
env # lists environmental variables
```

- 查找某正在运行进程的进程号

```bash
pidof watchdog
```

# Package管理

**opkg**

```bash
opkg list | grep ppp    #shows available modules grep filters out name
```

- 列出已安装

```bash
opkg list-installed
```

- 打个包查看

```bash
opkg info firewall
opkg files firewall
```

# web管理

```bash
opkg info luci
opkg info luci-ssl
```

# 查看DHCP客户端IP

```bash
cat /tmp/dhcp.leases
```

# 无线信息

- 帮助

```bash
iw help
```

- 获取设备的功能

使用以下方法来获得所有设备的功能，如带宽信息（2.4GHz，和5GHz），和802.11n的信息：

```bash
iw list
```

- 扫描无线路由器/AP

```bash
iw dev wlan0 scan
```

- 查看连接Openwrt路由的无线客户端：

```Bash
iwinfo wlan0 assoclist
iw dev wlan0 station dump
```

- 查看Openwrt路由无线信息：

```Bash
iwinfo wlan0 info
```

- 重启无线：

```Bash
wifi down && sleep 5 && wifi
```

- 一个bash脚本，输出当前无线客户端IP/MAC/名称/速率：

```Bash
#!/bin/bash
echo -e "# IP address\tname\tMAC address\ttx bit\trx bit"
for interface in `iw dev | grep Interface | cut -f 2 -s -d" "`
do
  maclist=(`iw dev $interface station dump | grep Station | cut -f 2 -s -d" "`)
  txlist=(`iw dev wlan0 station dump|grep 'tx bitrate'|awk '{print $3$4}'`)
  rxlist=(`iw dev wlan0 station dump|grep 'rx bitrate'|awk '{print $3$4}'`)
  len=${#maclist[@]}
  for ((i=0;i<$len;i++))
  do
    mac=${maclist[$i]}
    tx=${txlist[$i]}
    rx=${rxlist[$i]}
    ip="UNKN"
    host=""
    ip=`cat /tmp/dhcp.leases | cut -f 2,3,4 -s -d" " | grep $mac | cut -f 2 -s -d" "`
    host=`cat /tmp/dhcp.leases | cut -f 2,3,4 -s -d" " | grep $mac | cut -f 3 -s -d" "`
    echo -e "$ip\t$host\t$mac\t$tx\t$rx"|awk '{printf "%-15s %-25s %-15s %-10s %-10s\n",$1,$2,$3,$4,$5}'
  done
done
```

# 查看gpio口的信息

```bash
cat /sys/kernel/debug/gpio
```

# USB

- 查看USB连接设备

```bash
cat /sys/kernel/debug/usb/devices
```

# uci

**uci (command) [arguments]**

_Example:_

```bash
uci set dhcp.lan.leasetime=4h
uci get dhcp.lan.leasetime
uci show dhcp
uci changes
uci commit
```

# 其他

**swconfig** 配置交换机;

**iwinfo** 是OpenWrt中提供无线相关信息读取的软件.其中内置了诸如WEXT NL80211 madwifi等等驱动的各种信息读取方式.把不同类型的驱动整合出一个统一的接口供luci等使用;

**iwpriv iwlist iwconfig** 是WEXT类型无线驱动专用的操作程序.

ifconfig
iwconfig
pkill

cat /proc/modules
cat /proc/devices
cat /proc/iomem
cat /proc/cmdline
cat /proc/interrupts

cd /
find -name easycap
./sys/bus/usb/drivers/easycap
./sys/module/usbcore/holders/easycap
./sys/module/snd/holders/easycap
./sys/module/snd_pcm/holders/easycap
./sys/module/videodev/holders/easycap
./sys/module/easycap

sys adminname open
sys password open
sys multiuser on |backd backd
sys cwmp disp
