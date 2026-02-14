---
layout: post
title: 'xclip,pbcopy,xsel用法'
subtitle: '个使用经验总结'
date: 2015-01-15 21:28:00
comments: true
tags:
  - ubuntu
  - macOS
  - 备忘
---

### 1.Mac

使用**pbcopy**命令。 # 对应有个**pbpaste**命令。

示例：

```Bash
echo 'Hello World!' | pbcopy
```

将字符串Hello World放入剪贴板

```Bash
cat myFile.txt | pbcopy
pbpaste > file.txt
```

<!--more-->

_要复制结果又想看到命令的输出_
命令的结果输出时，如果给复制命令（即上面提到的命令clip、xsel、pbcopy）那么命令输出就看不到了。如果你想先看到命令的输出，可以下面这么做。

```Bash
$ echo 'Hello World!' | tee tmp.file.txt
Hello World!
$ xsel < tmp.file.txt
$ rm tmp.file.txt
```

即先使用**tee**命令把输出输到控制台和一个文件中。命令执行完成后，再把输出的内容放到剪贴板中。

### 2.Ubuntu

ubuntu下的用户可以只用apt-get来安装：

```Bash
sudo apt-get install xclip
```

其他发行版的用户可以选择自己的安装方式，也可以用源码编译安装，**xclip**项目的主页是：http://sourceforge.net/projects/xclip/
xclip可以将内容输出到‘X’的剪切板中，比如：

```Bash
echo "Hello, world" | xclip
```

执行这个命令后你就可以用鼠标中键来在X程序中将内容粘贴出来。但是更多的时候，我们需要不仅仅把内容输出到‘X’的剪切板中，而是希望可以在GUI程序 中用ctrl + v也可以粘贴（比如，输出到gnome的剪切板中），下面这段命令就可以让你将内容输出到gnome的剪切板中：

```Bash
echo "Hello, world" | xclip -selection clipboard
```

再在一个GUI程序中按下ctrl + v，看下是不是粘贴上去了呢？顺着这个命令，我也重新写了一下ifconfig，让它在执行后输入内容到终端的同时，也将ip地址输出到剪切板中，因为通常情况下，查看ifconfig就是为了获取机器的ip地址：

```Bash
alias ifconfig='/sbin/ifconfig && echo `/sbin/ifconfig | sed -n 2p | awk "{ print \\$2 }" | grep -o "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}"` | xclip -selection clipboard'
```

或者

```Bash
xclip -sel clip < file
```

此时你就可以在网页等编辑框CTRL+V了。

项目主页：http://sourceforge.net/projects/xclip/
命令man page: http://linux.die.net/man/1/xclip

### 3.Linux

使用**xsel**命令。

示例：

```Bash
cat README.TXT | xsel
cat README.TXT | xsel -b # 如有问题可以试试-b选项
xsel < README.TXT
# 将readme.txt的文本放入剪贴板
xsel -c
# 清空剪贴板
```

### 4.Windows

使用系统自带的**clip**命令。
\# 位于C:\Windows\system32\clip.exe。

<!--more-->

示例：

```Bash
echo Hello | clip
# 将字符串Hello放入Windows剪贴板
dir | clip
# 将dir命令输出（当前目录列表）放入Windows剪贴板
clip < README.TXT
# 将readme.txt的文本放入Windows剪贴板
echo | clip
# 将一个空行放入Windows剪贴板，即清空Windows剪贴板
```

注：不同系统使用不同的复制命令。避免用文本编辑器打开这个文件、选中文本、`CTRL + C`这样繁琐操作。
