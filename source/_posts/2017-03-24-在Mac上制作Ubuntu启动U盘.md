---
layout: post
title: '在Mac上制作Ubuntu启动U盘'
date: 2017-03-22 21:07
comments: true
tags:
  - macOS
  - ubuntu
---

Mac的`diskutil`命令非常强大，可以格式化U盘，也可以直接制作系统安装启动盘。

## 格式化

县列表看看你的U盘是哪个，千万别选错了。

```bash
diskutil list
sudo diskutil eraseDisk FAT32 UBUNTU18 MBRFormat /dev/disk5
```

## 卸载U盘

```bash
diskutil unmountDisk /dev/disk5
```

## 刻录

```bash
sudo dd if=./ubuntu-18.04.3-desktop-amd64.iso of=/dev/disk5 bs=1m
```

备选
`pv -petr ubuntu-18.04.3-desktop-amd64.iso | dd of=/dev/disk2 bs=1m`
