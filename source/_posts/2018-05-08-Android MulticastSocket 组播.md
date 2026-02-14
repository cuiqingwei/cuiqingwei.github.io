---
layout: post
title: 'Android MulticastSocket 组播'
date: 2018-05-08 12:12:08
comments: true
tags:
  - android
  - 电子书包
---

现象是MulticastSocket发送组播消息,一些安卓平板上可以发送、接收组播消息,个别安卓不可用。

<!--more-->

1.能正常发送接收的安卓平板,向组播地址发送了组播消息后,同时会向组播地址发送一个 **IGMPv2** 的协议

![](/img/20180508/7820e355de8f7e4065c08b677e8fb41b.jpg)

2.能发送,接收不到组播消息的安卓平板设备,可以发现设备没有向组播地址发送 **IGMP** 协议；

![](/img/20180508/394a6a000dd59316bab0f2d0d9b10387.jpg)

3.而查看安卓平板,在 **/proc/net/** 目录下,有igmp文件的设备会发送IGMP协议,能正常完成组播消息收发,而没有igmp文件的,就是不能完成组播消息收发的。**igmp** 是IPv4下的协议文件, **igmp6** 是IPv6的协议。可以看到所有设备都支持IPv6的igmp

![](/img/20180508/2b61d0bd7c6a7742160c4a7aa1fd85c8.jpg)

4.在Android系统下,编译Linux kernel时有一config CONFIG_IP_MULTICAST ,如果是定制系统在编译系统的时候配置这个选项 **CONFIG_IP_MULTICAST=y** ,就能enable组播功能。但是有很多系统厂商在编译的时候都默认把这个config给disable了。

```
CONFIG_IP_MULTICAST
adb shell netcfg
adb shell ifconfig eth0
```

> 参考:
> [How can I know if IP Multicast is enabled](https://unix.stackexchange.com/questions/25872/how-can-i-know-if-ip-multicast-is-enabled)
> [Android App用MulticastSocket监听组播，为什么连接到不同路由、在不同手机上跑，有的能收到有的收不到](https://www.cnblogs.com/inkheart0124/p/5508420.html)
> [Many devices have multicast disabled in the kernel](https://code.google.com/p/android/issues/detail?id=51195)
