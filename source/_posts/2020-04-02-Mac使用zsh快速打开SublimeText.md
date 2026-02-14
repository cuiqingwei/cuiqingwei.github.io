---
layout: post
title: 'Mac使用zsh快速打开SublimeText'
date: 2020-04-02 21:37
comments: true
tags:
  - 备忘
---

通过配置~/.zshrc加入alias来通过命令行启动sublime。

1. 打开 .zshr

```
vim ~/.zshrc
```

2. 在 .zshrc增加别名

```
alias subl="/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl"
```

其中 Sublime\ Text可以按照你系统上安装的Sublime版本的情况被替换成Sublime\ Text\ 2或是其他。取决于你的Sublime Text.app在~/Application里的实际名称。

3. 更新改动，生效

```
source ~/.zshrc
```

4. 即可使用sublime在命令行中创建和编辑文件

```
subl hello.c
```

5. 即可使用sublime在命令行中打开当前文件夹

```
subl .
```
