---
layout: post
title: 'OpenWRT下ifconfig各接口说明'
subtitle: 'ifconfig各接口说明'
date: 2015-07-06 18:00:00
tags:
  - OpenWRT
---

# rao 和raio（无线）

这两个是成对出现，一看就知道是无线设备，它们各自对应一个SSID，分别是2.4G和5G。

# eth0（物理网卡）

eth0.1 是vlan1分出的lan口。
eth0.2 是vlan分出的wan口。

<!--more-->

# br-lan（无线+有线）

虚拟设备，br-lan = eth0.1 + rai0 + ra0，即将有线LAN口和无线网统一划分为 LAN，便于管理

# lo

虚拟设备，自身的回环网设备。127.0.0.1

# aplic

虚拟设备，无线中继

# ppp0

虚拟设备，连接PPTP服务器后分配的虚拟ip

# pppoe-wan

虚拟设备，拨号上网的
