---
layout: post
title: 'Linux打补丁和git打patch方法 '
subtitle: 'usefull command'
date: 2015-08-19 14:50:00
comments: true
tags:
  - git
  - patch
  - linux
  - 备忘
---

# 一、linux打补丁方法

## 1、处理单个文件补丁的方法：

- 产生补丁

```
diff -uN from-file to-file >to-file.patch
```

- 打补丁

```
patch -p0 < to-file.patch
```

- 取消补丁

```
patch -RE -p0 < to-file.patch
```

<!--more-->

## 2、对整个文件夹打补丁的情况:

- 产生补丁

```
diff -uNr  from-docu  to-docu  >to-docu.patch
```

- 打补丁

```
cd to-docu
patch -p1 < to-docu.patch
```

- 取消补丁

```
patch -R -p1 < to-docu.patch
```

# 二、git打patch方法

## 1、git format-patch （推荐）

只适用于git的patch
包含diff信息，包含提交人，提交时间等
如果git format-patch 生成的补丁不能打到当前分支，git am会给出提示，并协助你完成打补丁工作
例：从master checkout一个新分支修改然后与master对比生成patch。

```
git format-patch -M master //-M选项表示这个patch要和那个分支比对
git am 001-xxx.patch（不必重新commit）
```

## 2、git diff

生成标准的patch，只包含diff信息
git diff生成的Patch兼容性强，可以用git apply --check 查看补丁是否能够干净顺利地应用到当前分支中。
例：从master checkout一个新分支修改然后与master对比生成patch。

```
git diff master > patch
git apply xxx.patch（需要重新commit）
```
