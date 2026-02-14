---
layout: post
title: "Hexo加Yilia搭建博客"
date: 2016-09-15 16:03
comments: true
tags:
  - hexo
  - yilia
---

# 1.搭建博客

## 1.1.本地安装Hexo

下载安装Git与Node.js略过

1.安装hexo

```
npm install hexo -g #-g表示全局安装, npm默认为当前项目安装
```

<!-- more -->

2.Hexo基本命令:

```
npm update hexo -g  #升级 
hexo init <folder>  #执行init命令初始化hexo到你指定的目录
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo generate       #自动根据当前目录下文件,生成静态网页 hexo g
hexo server         #运行本地服务 hexo s
hexo clean          #清理
hexo deploy         #部署 hexo d

hexo server         #Hexo会监视文件变动并自动更新，无须重启服务器
hexo server -s      #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
```

最常用的是：

```
hexo clean
hexo g
hexo d
hexo s
```

这个时候hexo s之后就可以在浏览器

```
http://localhost:4000/
```

这里看到hexo博客的基本样子了

## 1.2.将网站推送至Github

1.首先在本地告诉退给谁
打开本地博客本目录下的配置文件_config.yml
修改最后一行的配置：

```
deploy:
  type: git
  repository: https://github.com/cuiqingwei/cuiqingwei.github.io.git
  branch: master
```

2.上传github

```
hexo clean
hexo g
hexo d
```

若执行hexo g出错则执行

```
npm install hexo --save
```

若执行hexo d出错则执行

```
npm install hexo-deployer-git --save
```

错误修正后再次执行hexo g和hexo d上传到服务器。
若未关联GitHub，执行hexo d时会提示输入GitHub账号用户名和密码，即:

```
username for 'https://github.com':
password for 'https://github.com':
```

输入你的github账号密码即可。
`hexo d`执行成功后便可通过[https://cuiqingwei.github.io](https://cuiqingwei.github.io)访问博客

3.记住github密码
在github添加ssh key后不需要每次更新博客再输入用户名和密码。首先检查本地是否包含ssh keys。如果存在则直接将ssh key添加到GitHub之中，否则新生成ssh key。

执行下述命令生成新的ssh key

```
ssh-keygen -t rsa -C "your_email@exampl"
```

这里需要将

```
your_email@example.com
```

改成自己注册的GitHub邮箱地址。默认会在`~/.ssh/id_rsa.pub`中生成`id_rsa`和`id_rsa.pub`文件。

Mac下利用`open ~/.ssh`打开文件夹，打开`id_rsa.pub`文件，里面的信息即为ssh key，将此信息复制到GitHub的Add ssh key路径`GitHub->Setting->SSH keys->add SSH key`中即可。Title里填写任意标题，将复制的内容粘贴到key中，点击Add key完成添加即可记住密码。

此时本地博客内容便已关联到GitHub之中，本地博客改变之后，通过hexo g和hexo d便可更新到GitHub之中。

## 1.3.Github绑定域名

如果你有自己的域名了，比如买了“xxx.com”
那么可以把博客与域名关联起来，这样你打开xxx.com显示的就是xxx.github.io的内容。你打开xxx.github.io会自动跳转xxx.com

这里演示的是在阿里云万网的域名绑定，在国内主流的域名代理厂商也就阿里云和腾讯云。登录到阿里云，进入管理控制台的域名列表，找到你的个性化域名，进入解析，添加记录类型`CNAME`记录值`xxx.github.io`

然后再你本地博客文件夹下的source文件夹里添加一个文件，文件名为”CNAME”，内容为你的域名。
例如：`xxx.com`  这里要求一个字符不差。

## 1.4.更改主题（以Yilia为例）

> [hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia) 是为 [hexo](https://github.com/tommy351/hexo) 2.4+制作的主题。     
> 响应式设计，风格简约。

1. 安装yilia

   ```
   git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
   ```

2. 配置
   修改hexo根目录下的 _config.yml ： theme: yilia

   ```
   # Header
   menu:
     主页: /
     所有文章: /archives
     # 随笔: /tags/随笔
   
   # SubNav
   subnav:
     github: "https://github.com/litten"
     weibo: "http://weibo.com/litten225"
     rss: "http://feed.feedsky.com/litten"
     # facebook: "/"
     # google: "/"
     # twitter: "/"
     # linkedin: "/"
   
   rss: /atom.xml
   
   # Content
   excerpt_link: more
   fancybox: true
   
   # Miscellaneous
   
   favicon: /favicon.png
   
   avatar: "https://avatars2.githubusercontent.com/u/2024949?v=2&s=150"
   share: true
   duoshuo: true
   ```

3. 更新

   ```
   cd themes/yilia
   git pull
   ```

# 2.编写博客

## 2.1.Hexo文章的开头

举例：

```
title: 从零搭建博客 教程 Hexo GitHub    #文章标题
subtitle:                            #子标题
#categories: 文章                     #文章分類目錄 可以省略    
toc: true                            #目录 需要安装插件或者主题自带
reward: true                         #打赏 需要安装插件或者主题自带
declare: true                        #版权 需要安装插件或者主题自带
date:                                #文章日期 2019/7/1
header-img:                          #顶部背景图片
tags:                                #标签
- 自学
- Blog
- Original
```

## 2.2.Hexo文章的内容

### 2.2.1markdown常用语法

#### 2.2.1.1 标题

建立一个1级标题：

```
# 建立一个1级标题：
```

建立一个2级标题：

```
## 建立一个2级标题
```

建立一个3级标题：

```
### 建立一个3级标题
```

最多五级标题

#### 2.2.1.2建立一个超链接

效果:More info: [Writing](https://hexo.io/docs/writing.html)
代码如下：

```
More info: [Writing](https://hexo.io/docs/writing.html)

新窗口打开链接：
<a target = "_blank" href="#">AAA</a>
#：表示要跳转的连接
点击 AAA 超链接，将在新窗口打开#连接
```

#### 2.2.1.3建立目录

大部分Hexo主题不支持markdown建立目录，不过这里还是教一下。
代码如下：

```
[TOC]
```

#### 2.2.1.4建立标签

代码如下：

```
tags: 自学记录 (写在头上）

若多个标签，则：
tags:
- first
- essay
- picture
效果看题头
```

#### 2.2.1.5代码块

代码如下：

```
先写三个“`”带一个空格，后面写语言类型例如C++
再写三个“`”
```

**注意！**

```
写markdown的时候，用txt打开，一定要用UTF-8保存。否则中文乱码。
```

#### 2.2.1.6图片
代码如下：

```
![1](/img/2016-09-15.jpg)
或者
<img src="https://http://cuiqingwei.github.io/img/avatar.png" width = 100% div align=center/>
下面这个可以调整大小。
```

#### 2.2.1.7音乐

（具体功能百度htmlaudio标签）

```
<audio id="audio" autoplay="autoplay">
    <source src="http://qzone.haoduoge.com/music1/2015-04-23/1429774382.mp3" type="audio/mp3"></source>
</audio>
```

#### 2.2.1.8怎么改字体加颜色

摘要：
**加粗**

*斜体*

下划线

删除线

~~删除线2~~
DEMO1: 上标
DEMO2: 下标

居中
标记

```
<font color="red" face="宋体" size= 3>摘要：</font>
<b> 加粗 </b>
<I>斜体</I> 
<U>下划线</U> 
<S>删除线</S> 
<STRIKE>删除线2</STRIKE> 
DEMO1: <SUP>上标</SUP> 
DEMO2: <SUB>下标</SUB>
<!-- 注释 -->
<center>居中</center>
<mark>标记</mark>
```

#### 2.2.1.9怎么加空格和回车

```
空格&nbsp;&nbsp;&nbsp;&nbsp;
回车<br/> <br/>
```

#### 2.2.1.10折叠

<details style="box-sizing: border-box; display: block; color: rgb(26, 26, 26); font-family: &quot;lucida grande&quot;, &quot;lucida sans unicode&quot;, lucida, helvetica, &quot;Hiragino Sans GB&quot;, &quot;Microsoft YaHei&quot;, &quot;WenQuanYi Micro Hei&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-style: initial; text-decoration-color: initial;"><summary style="box-sizing: border-box; display: block;"><font color="darkred" style="box-sizing: border-box;">标题</font></summary></details>

```
<details>
  <summary><font color=darkred>标题</font></summary>
  被折叠内容
</details>

<details><summary><font color=darkred>标题</font></summary>
  被折叠内容</details>
```

#### 2.2.1.11页面内跳转

页面内跳转由两部分组成：
跳转标识（mark）：在你想跳转到的地方放一个标识，后面就会跳到这里来。

```
<div id="Mark"></div>
```



跳转命令：点击就会跳转到对应的标识

```
[返回目录](#Mark)
```

#### 2.2.1.12 文章省略

这里是为了不再博客主页面把每一个文章全部显示出来
在文章中间插入：

```
<!--more-->
```

其就会显示到标识前面的部分。

## 2.3 图床

*图床*就是网上可以在线储存图片的服务器。因为markdown里面无法存储图片，所以我们要把图片放在图床里，再以链接的形式引用。

**什么是好的图床**：（重要程度一次递减）
1.平台稳定（别动不动就崩 别用几天平台没了 ）
2.图片存储时间长（保证你的博客需求）
3.支持所需的图片要求（大小 格式要求）
4.加载速度
5.总容量
6.廉价最好免费

根据上面的几个指标利用知乎，百度去搜索吧~

# 参考链接

[GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)
[超详细Hexo+Github博客搭建小白教程](https://zhuanlan.zhihu.com/p/35668237)
[Mac+Hexo+GitHub搭建博客教程](https://zhuanlan.zhihu.com/p/34654952)