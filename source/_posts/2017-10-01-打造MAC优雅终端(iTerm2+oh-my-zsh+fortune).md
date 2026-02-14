---
layout: post
title: '打造MAC优雅终端(iTerm2+oh-my-zsh+fortune)'
date: 2017-10-01 18:26
comments: true
tags:
  - iTerm
---

![iterm2](/img/20171001/iterm2.png)

**iTerm2** + **oh-my-zsh** + **fortune** 用了很久了，换电脑有时会忘记当时是如何折腾的。整理个文件以作备忘！

> 完成配置后的终端具备功能：
>
> 1. 语法高亮;
> 2. 命令行`Tab`补全;
> 3. 自动提示符;
> 4. `⌘+r` 快速清空代码面板，跟终端输入`clear`一样的效果;
> 5. 每次打开，随机显示格言。
> 6. 终端查看图片、传输&下载文件等。

<!--more-->

## iTerm2

### 1. 下载[iTerm2](http://www.iterm2.com/)

### 2. 配置

1. 打开iTerm，在默认菜单中选择 `iTerm2 -> Make iTerm2 Default Term`
2. `⌘+,`打开偏好设置preference，选中Keys，勾选Hotkey下的Show/hide iTerm2 with a system-wide hotkey，将热键设置为 `⌘+.` ，这样你就可以通过 `⌘+`. 全局热键来打开或关闭iTerm2窗口，非常方便。
3. 更改配色方案：偏好设置 preference ，点开 profiles 下的colors 选项，点击右下角的 Color Presets 选项，选择import ，导入解压到的 solarized 文件下的Solarized Dark。

### 3. 增强

打开iTerm，点击 `iTerm2 -> Install Shell Intergration` 弹出窗口选择 `Install Shell Intergration & Utilities`按钮，完成后iTerm会支持一下命令。

> You will also have these commands:
> **imgcat** filename
> _Displays the image inline._
> **imgls**
> _Shows a directory listing with image thumbnails._
> **it2api**
> _Command-line utility to manipulate iTerm2._
> **it2attention** start|stop|fireworks
> _Gets your attention._
> **it2check**
> _Checks if the terminal is iTerm2._
> **it2copy** [filename]
> _Copies to the pasteboard._
> **it2dl** filename
> _Downloads the specified file, saving it in your Downloads folder._
> **it2setcolor** ...
> _Changes individual color settings or loads a color preset._
> **it2setkeylabel** ...
> _Changes Touch Bar function key labels._
> **it2ul**
> _Uploads a file._
> **it2universion**
> _Sets the current unicode version._

## oh-my-zsh

### 1. 安装

github：https://github.com/robbyrussell/oh-my-zsh
官方提供 curl 和 wget 两种安装方式

- curl 安装：
  `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
- wget安装：
  `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

### 2. 配置主题

#### 安装oh-my-zsh成功后

1. 用 vim 编辑隐藏文件 .zshrc， 终端输入

   `vi ~/.zshrc`

   将zsh主题修改为“agnoster”

   `ZSH_THEME="agnoster"`

2. 应用“agnoster”主题需要特殊的字体支持，否则会出现乱码情况，

#### 使用 [Meslo](https://github.com/powerline/fonts/blob/master/Meslo Slashed/Meslo LG M Regular for Powerline.ttf) 字体，点开链接点击 view raw 下载字体

1. 双击运行Meslo LG M Regular for Powerline.ttf 字体文件，点击`安装字体`
2. 在iTerm2中应用字体 iTerm -> Preferences -> Profiles -> Text -> Change Font）

## 增强功能

### 1. 语法高亮插件 zsh-syntax-highlighting

```
# 执行下面指令自动安装
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

### 2. 自动补全插件 zsh-autosuggestions

```
# 执行下面指令自动安装
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

### 3. 自动跳转插件 autojump

cd 过一次的目录下次都可以直接`j 目录`跳转

```
# clone 到本地
git clone https://github.com/wting/autojump.git
# 进入clone目录，接着执行安装文件
cd autojump
./install.py
# 接着根据安装完成后的提示，在~/.bashrc最后添加下面语句：
vim ~/.zshrc
[[ -s /home/cuiqingwei/.autojump/etc/profile.d/autojump.sh ]] && source /home/cuiqingwei/.autojump/etc/profile.d/autojump.sh
autoload -U compinit && compinit -u
```

#### 4. 安装完后启用插件

```
# 编辑~/.zshrc
vim ~/.zshrc
# 在plugins后括号里添加安装的插件名字
plugins=(   git
            autojump
            zsh-autosuggestions
            zsh-syntax-highlighting
            )
在~/.bashrc最后添加下面语句
source ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
# 最后刷新
source ~/.zshrc
```

> autojump 工作原理：它会在你每次启动命令时记录你当前位置，并把它添加进它自身的数据库中。这样，某些目录比其它一些目录添加的次数多，这些目录一般就代表你最重要的目录，而它们的“权重”也会增大。

```
查看版本：
j -v
目录跳转：
 j    [目录的名字或名字的一部分] // 不受当前所在目录的限制

查看当前权重：
j --stat

进入权重最高的目录：
j

改变当前目录权重值：
j -i [权重] // 增加
j -d [权重] // 减少
```

# fortune

```bash
$ brew install fortune
```

我很喜欢这个程序，但是它默认的格言库都是英语的，我觉得这点不好，就自己动手做了一个[中文格言库](https://github.com/ruanyf/fortunes)。上图就来自这个库，安装命令如下。

```bash
$ git clone https://github.com/ruanyf/fortunes.git

$ sudo mv fortunes/data/* /usr/share/games/fortunes/
```

如果是Mac系统，需要重新生成索引文件，即在上面两个命令之间，增加几个命令。

```bash
$ strfile fortunes/data/fortunes
$ strfile fortunes/data/chinese
$ strfile fortunes/data/tang300
$ strfile fortunes/data/song100
```

目前，这个库带有四个格言包。

- fortunes：英语格言，5472条
- chinese：中文格言，25919条
- tang300：唐诗三百首，313条
- song100:：宋词一百首，95条

fortune 命令显示的格言是随机的，但是分布比例与格言包的容量一致。如果希望四个包平均显示，即每个包都有25%的显示机会，需要使用下面的命令格式。

```bash
$ fortune -e fortunes chinese tang300 song100
# 或者
$ fortune 25% fortunes 25% chinese 25% tang300 25% song100
```

上面代码的第二种形式，让你可以根据自己的偏好，调节每个包的比重。

在 ~/.bashrc 或 ~/.zshrc 文件（根据你使用的 shell 而定）的结尾，加上下面几行，那么每次启动 shell 窗口，就会自动跳出一句格言。

```bash
echo
echo "=============== Quote Of The Day ==============="
echo
fortune
echo
echo "================================================"
echo
```

<hr>

# 或许有用

1. 查看shell：

`cat /etc/shells`

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh

2. 切换shell：

`chsh -s /bin/zsh`
`chsh -s /bin/bash`

3. 查看当前shell，但不能时时反映shell，需重启iTerm2：

`echo $SHELL`

4. 卸载`oh my zsh`，在命令行输入：`uninstall_oh_my_zsh`

5. 路径前缀的XX@XX太长，缩短方法：
   编辑`~/.oh-my-zsh/themes/agnoster.zsh-theme`主体文件，将里面的`build_prompt`下的`prompt_context`字段在前面加`#`注释掉即可。
