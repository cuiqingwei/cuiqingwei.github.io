---
layout: post
title: 'Linux中生成shadow的加密密码'
date: 2012-09-07 15:50
comments: true
tags:
  - linux
---

How to generate Linux shadow password / Linux中如何生成shadow密码（CentOS 5下测试通过）
/etc/shadow中的root信息:
root:$1$nnXLnQUR$eqQTkzEx/xP2yvVDyb6jO.:15225:0:99999:7:::

其中，”$1$nnXLnQUR$eqQTkzEx/xP2yvVDyb6jO.” 就是加密后的root密码，我们在修改用户密码的时候都使用passwd命令，其实也可以直接修改/etc/shadow文件。那要面临的问题就是如何生成这个加密过的shadow密码($1$ 表示加密算法是基于MD5的，不同发行版本默认的加密算法可能不同)。

方法一（交互式）
[root@centos-vm ~]# openssl passwd -1 -salt $(< /dev/urandom tr -dc '[:alnum:]' | head -c 32)
Password:    # input your password here
$1$MZrDxgEw$i2XBgmDMo0Wf1.OVvOGGQ1

方法二（适用于脚本）
[root@centos-vm ~]# echo "passwd" | openssl passwd -1 -salt $(< /dev/urandom tr -dc '[:alnum:]' | head -c 32) -stdin
$1$KsRJO8kG$M9co4G7T6.5KcITsSCRNS/
