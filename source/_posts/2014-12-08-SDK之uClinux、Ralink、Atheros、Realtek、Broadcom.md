---
layout: post
title: 'SDK之uClinux、Ralink、Atheros、Realtek、Broadcom'
date: 2014-12-08 23:34
comments: true
tags:
  - 路由器SDK
  - 嵌入式
---

接触的Linux SDK越来越多，整理整理，分享分享，不求系统全面，对您有帮助便足矣
文中大部分是与AP/Router SoC解决方案（单芯片WIFI 路由器解决方案）相关的Linux SDK

# SDK（Software Development Kit）软件开发工具包

一般是一些被软件工程师用于为特定的软件包、软件框架、硬件平台、操作系统等建立应用软件的开发工具的集合
想想，最早接触到的SDK是在Cortex-M3项目开发中，也叫BSP（板级支持包），
这种SDK里大部分是芯片资源（硬件平台）的驱动和测试程序，少有软件框架和操作系统的源码

<!--more-->

# uClinux

在用的ATMEL9200和9G25的SDK包就是uClinux的风格，Ralink的Ralink_Ap_SoC_SDK也是uClinux的风格，
不用多说，直接上地址：http://www.uclinux.org/

# Ralink平台

在用他们的AP/Router SoC方案，比如RT3050/3052，对应SDK的版本是Ralink_Ap_SoC_SDK_3301.tar.bz2，应该是从uClinux基础上修改而来，
SDK的下载地址：http://www.tw-mcse.net/mcic/（重新更新下载地址），http://www.tw-mcse.net/mcic/Ralink_ApSoC_SDK_3301.tar.bz2
华硕的RT-N56U/RT-N65U有在google code上公开源码，地址：https://code.google.com/p/rt-n56u/

# Atheros平台

也在用他们的AP/Router SoC方案，比如AR9331/9344，说起Atheros不得不提TP-Link，TP-Link大量使用Atheros的方案，其中TL-WDR4300/4310就是用的AR9344，
TP-Link根据GPL协议开放了一些源码，地址：http://www.tp-link.com/en/support/gpl/
比如下载TL-WDR4300的源码包（GPL_2.6.31），是在Atheros官方提供的SDK包（LSDK-9.2.0和LSDK-WLAN-9.2.0）基础上进行了修改，但是没有开放LSDK-WLAN-9.2.0，不过没关系，LSDK-WLAN-9.2.0在CSDN上可以下载，地址：http://download.csdn.net/detail/nolycjyf/3722308
对于Atheros的WLAN驱动，另插一句，还记得madwifi、ath9k吗，没错，是同族的

# Realtek平台

因为芯片封装上都有一只大螃蟹，所以大家喜欢叫他螃蟹，
在sourceforge上找到了它的一个SDK（RTL819x-SDK-v3.2.3），也是AP/Router SoC方案，感兴趣的可以下下来看看，
地址：http://sourceforge.net/projects/rtl819x/?source=directory

# Broadcom平台

想换成这个平台，这个平台的SDK资料相对少一点，但还是找到了一点踪迹，
华硕的RT-N16有在google code上开放源码，RT-N16用的就是Broadcom的BCM4716
地址：https://code.google.com/p/rt-n16/

遗憾是没有找到任何有关Marvell平台的SDK资料
找了这么多SDK源码包拿来干什么？
首先学习软件框架，Makefile会告诉你真相
然后就是Linux驱动，尤其WLAN/WIFI驱动
