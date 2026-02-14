---
layout: post
title: 'OpenWRT的WiFi带机量修改方法'
date: 2018-05-14 12:12:08
comments: true
tags:
  - OpenWRT
  - 电子书包
---

WiFi带机量的修改

(1)修改MAX_NUMBER_OF_MAC(rt2860v2/include/oid.h)

```
#define MAX_NUMBER_OF_MAC                64 // 原始值32
```

(2)需要同时增大RX_RING_SIZE和TX_RING_SIZE，否则连接客户端一多就会掉包(rt2860v2/include/os/rt_linux_cmm.h中)

<!--more-->

> 参考:
> [MT7620的WIFI带机量](https://blog.csdn.net/mcusun2000/article/details/50875488)
> [MTK_Ralink_ApSoc_SDK编译总结](https://blog.csdn.net/manfeel/article/details/38356727)
> [openwrt wifi用户连接数限制](http://www.right.com.cn/forum/thread-208204-1-1.html)
