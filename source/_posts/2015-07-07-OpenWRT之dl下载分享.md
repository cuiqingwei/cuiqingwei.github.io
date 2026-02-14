---
layout: post
title: 'OpenWRT之dl下载分享'
subtitle: 'OpenWRT学习、研究、开发记录'
date: 2015-07-07 18:00:00
tags:
  - OpenWRT
---

在编译OpenWrt最新的trunk时，碰到一个Git源码取不下来（你懂得，被墙了！）
在google上搜了一下，找到一个dl的地址：
http://193.87.95.148/openwrt/raspi/trunk/dl/
补充：OpenWrt中国站也有一个下载地址！
http://downloads.openwrt.org.cn/sources/

另外还有一个将openwrt官网上的软件都下载下来了，并在本地建了服务器，然后与共享给大家！！
跳转下载链接http://dl.openwrtdl.com/index.html
软件包总数量达到了**3926**个！
最新更新时间：**2016-05-09**

<!--more-->

并用脚本每小时执行一次，与openwrt官网对比，当官网更新了软件包则将对应的软件包下载到本地，保证官网更新，本地也会更新的效果！

服务器在我家里放着，上海长城宽带，20M光纤，上传速度可以达到3MB，对于下载小的软件包足够了！！

另外我会写个脚本每个一小时与openwrt官网同步一次，发现更新则加入到本地！！

欢迎大家来下载，如果有哪些软件未包含进来，请留言给我！！！

====================================================

我把所有的软件都放进了百度云盘下载，本文章中也会有所有的软件包的列表

只要你在网页内用“ctrl+f”搜索你需要的软件包就可以下载了！

当然也可以先进入百度分享连接中，然后“ctrl+f”搜索，刚发现个问题，百度网盘中并不是一次性的加载完所有的软件包列表，而是加载一部分，当你把滚动条拉到最底时他才会加载新的，这很蛋疼。。。体验就不好了！

网盘分享连接：http://pan.baidu.com/s/1dD4Rl53

另外分享几个国外的openwrt软件源下载的地方，当然速度不一定好

http://mirror2.openwrt.org/sources/ （openwrt官方网站，服务器在匈牙利，目前已经停止更新）

https://downloads.openwrt.org/sources/ （openwrt官方网站，服务器在匈牙利）

http://downloads.openwrt.org.cn/sources/ （国内dreambox和pandora团队建立的，服务器在日本）

http://143.129.80.193/openwrt/dl/ （比利时的一个网站上，资源比官方的少，目前已无法访问）

TPLINK-WR703的trunk的dl

http://log.nihon-ryouri.asia/tpl703n/trunk/dl/ （服务器在法国）

========感谢leejon提供的统计数据========

软件包条目数：
http://pan.baidu.com/s/1dD4Rl53 2022个
http://mirror2.openwrt.org/sources 3860个
https://downloads.openwrt.org/sources 2672个
http://downloads.openwrt.org.cn/sources 3223个
http://143.129.80.193/openwrt/dl 590个

原文链接 http://www.openwrtdl.com/wordpress/package-index-download
