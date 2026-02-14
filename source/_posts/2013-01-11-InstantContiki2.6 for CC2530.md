---
layout: post
title: 'InstantContiki2.6 for CC2530'
subtitle: 'InstantContiki'
date: 2013-1-11 13:14:34
tags:
  - Contiki
  - CC2530
  - 6lowpan
---

搞zigbee算是国内第一批了，很早就知道6lowpan。用cc2430时，从研究[http://www.tinyos8051wg.net](http://www.tinyos8051wg.net)，找寻资料了解到 Nanostack1.0.0，直到Nanostack1.1.0 之后就转为contiki了。好久没发帖了，发个帖Mark一下，先！

1.Download Instant Contiki

[<font color="#000080">http://sourceforge.net/projects/contiki/files/Instant%20Contiki/</font>](http://sourceforge.net/projects/contiki/files/Instant%20Contiki/)

2.Install VMWare Player

[<font color="#000080">https://my.vmware.com/web/vmware/free#desktop_end_user_computing/vmware_player/5_0</font>](https://my.vmware.com/web/vmware/free#desktop_end_user_computing/vmware_player/5_0)

3.Log into Instant Contiki.

password is: **<font color="red">user</font>**

<!--more-->

4.编译~/contiki/examples/cc2530dk下例子，需要先安装SDCC.

&nbsp;&nbsp;参考文档

&nbsp;&nbsp;[<font color="#000080">https://github.com/contiki-os/contiki/wiki/8051-Requirements</font>](https://github.com/contiki-os/contiki/wiki/8051-Requirements)

&nbsp;&nbsp;4.1）下载SDCC

<font color="blue">&nbsp;&nbsp;cd /opt

&nbsp;&nbsp;sudo svn co -r 7100 </font>[<font color="blue">&nbsp;&nbsp;https://sdcc.svn.sourceforge.net/svnroot/sdcc/trunk/sdcc/</font>](https://sdcc.svn.sourceforge.net/svnroot/sdcc/trunk/sdcc/)

&nbsp;&nbsp;4.2) 更改配置

&nbsp;&nbsp;<font color="blue">sudo vi /opt/sdcc/device/libs/incl.mk</font>

&nbsp;&nbsp;将最后一行

&nbsp;&nbsp;MODELS = small medium large

&nbsp;&nbsp;替换为

&nbsp;&nbsp;**MODELS = small <font color="red">large huge</font>**

&nbsp;&nbsp;<font color="blue">sudo vi /opt/sdcc/device/libs/Makefile.in</font>

&nbsp;&nbsp;将最后一行

&nbsp;&nbsp;TARGETS += models small-mcs51-stack-auto

&nbsp;&nbsp;替换为

&nbsp;&nbsp;**TARGETS += models <font color="red">model-</font>mcs51-stack-auto**

&nbsp;&nbsp;<font color="blue">cd /opt/sdcc</font>

&nbsp;&nbsp;•Run this:

&nbsp;&nbsp;<font color="blue">sudo ./configure --disable-gbz80-port --disable-z80-port --disable-ds390-port \</font>

&nbsp;&nbsp;<font color="blue">--disable-ds400-port --disable-pic14-port --disable-pic16-port \</font>

&nbsp;&nbsp;<font color="blue">--disable-hc08-port --disable-r2k-port --disable-z180-port \</font>

&nbsp;&nbsp;<font color="blue">--disable-sdcdb --disable-ucsim</font>

&nbsp;&nbsp;此过程若出现

&nbsp;&nbsp;_configure: error: boost library not found (boost/graph/adjacency_list.hpp)_

&nbsp;&nbsp;执行

<font color="red">&nbsp;&nbsp;sudo apt-get install libboost-graph-dev</font>

&nbsp;&nbsp;完成后，重新执行

&nbsp; &nbsp;<font color="blue">sudo ./configure --disable-gbz80-port --disable-z80-port --disable-ds390-port \</font>

&nbsp;&nbsp;<font color="blue">--disable-ds400-port --disable-pic14-port --disable-pic16-port \</font>

&nbsp;&nbsp;<font color="blue">--disable-hc08-port --disable-r2k-port --disable-z180-port \</font>

&nbsp;&nbsp;<font color="blue">--disable-sdcdb --disable-ucsim\_</font>

&nbsp;&nbsp;4.3）编译，安装

<font color="blue">&nbsp;&nbsp;sudo make

&nbsp;&nbsp;sudo make install</font>

&nbsp;&nbsp;4.4）完成

<font color="blue">&nbsp;&nbsp;sdcc -v</font>

&nbsp;&nbsp;显示

&nbsp;&nbsp;_SDCC : mcs51 3.1.1 #7100 (Jan 10 2013) (Linux)_

5.编译cc2530dk

&nbsp;&nbsp;5.1) hello world

&nbsp;&nbsp;<font color="blue">cd ~/contiki/examples/cc2530dk</font>

&nbsp;&nbsp;<font color="blue">make hello-world</font>

&nbsp;&nbsp;编译完成

&nbsp;&nbsp;<font color="#000080">user@instant-contiki:~/contiki/examples/cc2530dk$</font> ls

&nbsp;&nbsp;blink-hello.c&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;hello-world.c&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;obj_cc2530dk&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;timer-test.c

&nbsp;&nbsp;blink-hello.cc2530dk&nbsp;&nbsp;hello-world.cc2530dk&nbsp;&nbsp;sensors-demo.c&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;timer-test.cc2530dk

&nbsp;&nbsp;blink-hello.cdb&nbsp; &nbsp;&nbsp; &nbsp; hello-world.cdb&nbsp; &nbsp;&nbsp; &nbsp; sensors-demo.cc2530dk&nbsp;&nbsp;timer-test.cdb

&nbsp;&nbsp;blink-hello.hex&nbsp; &nbsp;&nbsp; &nbsp; hello-world.hex&nbsp; &nbsp;&nbsp; &nbsp; sensors-demo.cdb&nbsp; &nbsp;&nbsp; &nbsp; timer-test.hex

&nbsp;&nbsp;blink-hello.lk&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;hello-world.lk&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;sensors-demo.hex&nbsp; &nbsp;&nbsp; &nbsp; timer-test.lk

&nbsp;&nbsp;blink-hello.map&nbsp; &nbsp;&nbsp; &nbsp; hello-world.map&nbsp; &nbsp;&nbsp; &nbsp; sensors-demo.lk&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;timer-test.map

&nbsp;&nbsp;blink-hello.mem&nbsp; &nbsp;&nbsp; &nbsp; hello-world.mem&nbsp; &nbsp;&nbsp; &nbsp; sensors-demo.map&nbsp; &nbsp;&nbsp; &nbsp; timer-test.mem

&nbsp;&nbsp;blink-hello.omf&nbsp; &nbsp;&nbsp; &nbsp; hello-world.omf&nbsp; &nbsp;&nbsp; &nbsp; sensors-demo.mem&nbsp; &nbsp;&nbsp; &nbsp; timer-test.omf

&nbsp;&nbsp;border-router&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;Makefile&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;sensors-demo.omf&nbsp; &nbsp;&nbsp; &nbsp; udp-ipv6

&nbsp;&nbsp;contiki-cc2530dk.lib&nbsp;&nbsp;Makefile.target&nbsp; &nbsp;&nbsp; &nbsp; sniffer

&nbsp;&nbsp;5.2) udp-ipv6

<font color="blue">&nbsp;&nbsp;cd ~/contiki/examples/cc2530dk/udp-ipv6</font>

<font color="blue">&nbsp;&nbsp;make</font>

&nbsp;&nbsp;报错，缺少**<font color="red">srecord</font>**

&nbsp;&nbsp;Pack hex file

&nbsp;&nbsp;===============

&nbsp;&nbsp;srec_cat -disable_sequence_warnings client.banked-hex -intel -crop 0x18000 0x1FFFF -offset -65536 -o bank1.hex -intel

&nbsp;&nbsp;/bin/sh: 5: srec_cat: not found

&nbsp;&nbsp;srec_cat -disable_sequence_warnings client.banked-hex -intel -crop 0x28000 0x2FFFF -offset -98304 -o bank2.hex -intel

&nbsp;&nbsp;/bin/sh: 5: srec_cat: not found

&nbsp;&nbsp;srec_cat -disable_sequence_warnings client.banked-hex -intel -crop 0x38000 0x3FFFF -offset -131072 -o bank3.hex -intel

&nbsp;&nbsp;/bin/sh: 5: srec_cat: not found

&nbsp;&nbsp;srec_cat -disable_sequence_warnings client.banked-hex -intel -crop 0x48000 0x4FFFF -offset -163840 -o bank4.hex -intel

&nbsp;&nbsp;/bin/sh: 5: srec_cat: not found

&nbsp;&nbsp;make: \*\*\* [client.hex] Error 127

&nbsp;&nbsp;执行

&nbsp;&nbsp;<font color="blue">sudo apt-get install srecord</font>

6.TI SmartRF Flash Programmer 烧录 \*.hex

&nbsp;&nbsp;已烧录hello-world.hex为例，CC2530 UART0 会打印输出如下信息：

&nbsp;&nbsp;##########################################

&nbsp;&nbsp;Contiki-2.6

&nbsp;&nbsp;TI SmartRF05 EB

&nbsp;&nbsp;cc2530-F256, 08KB SRAM

&nbsp;&nbsp;SDCC Build:

&nbsp; &nbsp; --model-large

&nbsp; &nbsp; --stack-auto

&nbsp; &nbsp;Net: Rime

&nbsp; &nbsp;MAC: CSMA

&nbsp; &nbsp;RDC: nullrdc

&nbsp;&nbsp;##########################################

&nbsp;&nbsp;Rime is 0x02 bytes long

&nbsp;&nbsp;Reading MAC from Info Page

&nbsp;&nbsp;Rime configured with address c6:1c

&nbsp;&nbsp;Rime is 0x02 bytes long

&nbsp;&nbsp;Reading MAC from Info Page

&nbsp;&nbsp;Rime configured with address 67:af

<font color="magenta">&nbsp;&nbsp;Hello World!</font>

7.建立自己的硬件平台[我的是：edutech]和工程[我的是：edutech]

&nbsp;&nbsp;7.1) cpu

&nbsp;&nbsp;<font color="blue">cd ~/contiki/cpu/cc253x

&nbsp;&nbsp;cp Makefile.cc253x Makefile.cc253x-edutech

&nbsp;&nbsp;cp bank-alloc.py bank-alloc-edutech.py</font>

<font color="blue">&nbsp;&nbsp;vi Makefile.cc253x-edutech</font>

&nbsp;&nbsp;编辑第14行

&nbsp;&nbsp;BANK_ALLOC = $(CONTIKI_CPU)/bank-alloc.py

&nbsp;&nbsp;为

&nbsp;&nbsp;**BANK_ALLOC = $(CONTIKI_CPU)/<font color="red">bank-alloc-edutech.py</font>**

&nbsp;&nbsp;保存

<font color="blue">&nbsp;&nbsp;:wq</font>

<font color="blue">&nbsp;&nbsp;vi bank-alloc-edutech.py</font>

&nbsp;&nbsp;编辑第101行

&nbsp;&nbsp;file_pat = re.compile('obj_cc2530dk[^ ]+\.')

&nbsp;&nbsp;为

** file_pat = re.compile(<font color="red">'obj_edutech</font>[^ ]+\.')**

&nbsp;&nbsp;将文中4处，157、158、172、173行中

&nbsp;&nbsp;bank-alloc.py 替换为 **<font color="red">bank-alloc-edutech.py</font> **

&nbsp;&nbsp;保存

<font color="blue">&nbsp;&nbsp;:wq</font>

&nbsp;&nbsp;7.2) platform

<font color="blue">&nbsp;&nbsp;cd ~/contiki/platform

&nbsp;&nbsp;cp cc2530dk edutech -R

&nbsp;&nbsp;cd edutech

&nbsp;&nbsp;mv Makefile.cc2530dk&nbsp;&nbsp;Makefile.edutech

&nbsp;&nbsp;vi Makefile.edutech</font>

&nbsp;&nbsp;编辑第1行[非必须]

&nbsp;&nbsp;# cc2530dk platform makefile

&nbsp;&nbsp;为

**&nbsp;&nbsp;# <font color="red">edutech</font> platform makefile**

&nbsp;&nbsp;编辑第19行

&nbsp;&nbsp;CLEAN += \*.cc2530dk

&nbsp;&nbsp;为

&nbsp;&nbsp;CLEAN += <font color="red">_.edutech _.native

</font>&nbsp;&nbsp;编辑第52行,最后一行

&nbsp;&nbsp;include $(CONTIKI_CPU)/Makefile.cc253x

&nbsp;&nbsp;为

&nbsp;&nbsp;**include $(CONTIKI_CPU)/<font color="red">Makefile.cc253x-edutech</font>**

&nbsp;&nbsp;保存

<font color="blue">&nbsp;&nbsp;:wq</font>

&nbsp;&nbsp;接下来根据实际硬件情况，编辑或创建驱动文件

&nbsp;&nbsp;7.3) project

<font color="blue">&nbsp;&nbsp;cd ~/contiki/examples

&nbsp;&nbsp;cp cc2530dk/ edutech -R

&nbsp;&nbsp;cd edutech

&nbsp;&nbsp;vi Makefile.target</font>

&nbsp;&nbsp;编辑第1行

&nbsp;&nbsp;TARGET = cc2530dk

&nbsp;&nbsp;为

&nbsp;&nbsp;TARGET = <font color="red">edutech</font>

&nbsp;&nbsp;保存

<font color="blue">&nbsp;&nbsp;:wq</font>

<font color="blue">&nbsp;&nbsp;make</font>

&nbsp;&nbsp;烧录

![programmer](/img/20130111/1.jpg)

# using TI CC Debugger (based on Ubuntu)

install Package libboost-all-dev and libusb-1.0-0-dev by using command

sudo apt-get install libboost-all-dev

sudo apt-get install libusb-1.0-0-dev

- download cc-tool source code from https://github.com/misham/cc-tool

- extract cc-tool source code and install by using command

  sudo wget https://github.com/misham/cc-tool/archive/master.zip

  sudo unzip master.zip

  cd cc-tool- master

  sudo ./configure

  make

  make install

- use lsusb command for list your Board

  lsusb

- the output should be

..Bus 006 Device 015: ID 0451:16a0 Texas Instruments, Inc. Bus 006 Device 013: ID 0451:16a0 Texas Instruments, Inc. ..

- change directory to your .hex file

  (example /home/user/contiki-sensinode/examples/cc2530dk)

  cd /home/user/contiki-sensinode/examples/cc2530dk

- start program board (cc-tool -ew FILE_NAME.hex -d'Bus:Device' -v)

  cc-tool -ew blink-hello.hex -d'006:015' -v

- the result should be

Programmer: SmartRF05EB Target: CC2530 Erasing flash... Erasing completed Writing flash... (42 KB)... Writing completed (3.13 s.) Verifying flash... Verification completed (3.33 s.)

- for more cc-tool command option you can use –help

  cc-tool –help

可使用 **<font color="red">sudo cc-tool –n cc2530 –w blink-hello.hex –v</font>** 烧录

#sensniff

[https://github.com/g-oikonomou/sensniff](https://github.com/g-oikonomou/sensniff)

[https://github.com/jerusalemdax/contiki-cc2530eb](https://github.com/jerusalemdax/contiki-cc2530eb)

下载

[git clone git://github.com/jerusalemdax/contiki-cc2530eb.git](git clone git://github.com/jerusalemdax/contiki-cc2530eb.git)

---

_发表链接_

amoBBS 阿莫电子论坛 [http://www.amobbs.com/thread-5515866-1-1.html](http://www.amobbs.com/thread-5515866-1-1.html)

www.iotdev.net [http://www.iotdev.net/thread-210-1-1.html](http://www.iotdev.net/thread-210-1-1.html)
