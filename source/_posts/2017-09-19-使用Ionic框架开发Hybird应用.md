---
layout: post
title: '使用Ionic框架开发Hybird应用'
date: 2017-09-19 00:09:08
comments: true
tags:
  - Hybird
  - Ionic
---

# 什么是Ionic

Ionic是一个强大的HTML5 原生应用（Native App）开发框架，帮助你用HTML，CSS和JavaScript构建具有原生味道的移动应用;

Ionic是一个高级HTML5混合式移动应用开发框架，同时也是一个开源的前端框架;

Ionic注重的是视觉效果和用户体验，所以使用了AngularJS来构建很各种酷的效果;

Ionic是类似Bootstrap概念的WebApp框架,提供了Hybrid App的基础CSS,UI控件,工程架构和最佳实践。

<!--more-->

# Ionic的优势

- 性能优异
- 基于红的发紫AngularJs
- 漂亮的UI
- 强大的命令行(基于更热门的NodeJS)
- 开发团队非常活跃，
- 相关配套非常齐全：
- 相对充足的学习资料，Learn Iconic,The Iconic book
- ngcordova，将主流的Cordova API或者Cordova插件封装为AngularJS扩展，使用非常方便。
- 开源免费的webfont icon库ionicons,基本满足你icon需求。
- 甚至最近开发出的可视化开发工具Ionic Creator
- 最后便是至关重要的，异常活跃的在线社区。

# 谈谈 Hybrid app

Ionic+Cordova的组合是一个跨平台的移动开发框架，属于HybirdApp开发模式。其中Ionic是一个前端框架，集成了AngularJs在里面，有很好很漂亮的UI控件。Cordova本身就是一个跨平台的移动开发框架，可以将应用打包成各个平台下的应用。他们之间的关系可以用下面的图展示：

![](/img/20170919/webapp0.png)

从网下图，可以很直观的看出Native app 、Web app和Hybrid app三者之间的区别：

![](/img/20170919/webapp1.png)
![](/img/20170919/webapp2.png)

- 1）Native APP：Native \*
  Code编程，代码编译之后以2进制或者字节码的形式运行在OS上，直接调用OS的Device API；

- 2）Web APP \* ，以HTML+JS+CSS等WEB技术编程，代码运行在浏览器中，通过浏览器来调用Device API（取决于HTML5未来的支持能力）：

- 3）Hybrid APP \* ，部分代码以WEB技术编程，部分代码由某些Native Container承担（例如PhonGAP插件，BAE插件），其目的是在HTML5尚未完全支持Device API和Network API的目前阶段，承担这部分职责。

总体来讲，** Hybrid App是同时采用网页语言与程序语言进行开发，通过不同的应用商店进行打包与分发，应用的特性更接近原生应用而且又区别与Web应用 ** 。但是在开发过程中同时使用了网页语言，所以开发成本与难度大大降低。也就是说Hybrid App兼具了Native App与Web App两者的诸多优点。

Hybrid App有以下的特性：

> 开发时可能不采用或者大部分不采用原生语言，但是却有所有原生应用的特性；
> 架构方案会和原生有出入，基本由工具而定；
> 具有跨平台特性；
> 一般开发相对原生开发的方式要简单。

工具方面，我们可以使用Angular.js用于双向数据绑定和网络请求以及视图管理等工作，使用require.js来实现模块化开发，组织代码.

# 安装Cordova和Ionic

如果你想要开始Ionic开发，你需要先安装Node.js。

> 在Win系统中：
> **npm install -g cordova ionic**
> 在Mac中，需要加上sudo：
> **sudo npm install -g cordova ionic**
> 安装完后在cmd或者终端输入ionic：

![](/img/20170919/webapp3.png)

这样就表示安装成功。

# 开始一个Ionic App

Ionic官网http://ionicframework.com为开发者提供了多个开发模板，如：

> 空白模板（Black app）：
> **ionic start myApp blank**
> tabs模板 ：
> **ionic start myApp tabs**
> sidemenu模板：  
> **ionic start myApp sidemenu**

![](/img/20170919/webapp4.png)

# 开始创建一个工程：

ionic start myApp1 tabs

# 运行

cd myApp1
ionic cordova platform add ios
ionic cordova build ios
ionic cordova emulate ios
然后再输入下面这串命令行，继续刷屏：
sudo npm install -g ios-sim
然后继续输入：
ionic cordova emulate ios
就会弹出模拟器了。

![](/img/20170919/webapp5.png)

我推荐大家使用**ionic serve**命令来使用电脑浏览器调试，如图：

![](/img/20170919/webapp6.png)

# Ionic插件安装与卸载

使用下面的命令查询、安装、卸载插件：
ionic plugin list //列出所有已安装插件
ionic plugin remove 插件名 //先根据上面的list列出插件，然后根据插件名卸载
ionic plugin add 插件地址 //这个插件地址可以是github的项目地址，也可以是一个文件夹路径
ionic –help //查看帮助文件
ionic项目托管平台地址： https://github.com/driftyco/ionic/
css组件：http://www.ionicframework.com/docs/components/#header

# 参考

Ionic+Cordova开发环境搭建 http://blog.csdn.net/gameloft9/article/details/50923615
使用ionic框架开发移动hybrid应用 https://segmentfault.com/a/1190000002688632
ionic react-native和native开发移动app那个好 ? http://www.phonegap100.com/article-486-1.html
ionic工作室 http://www.ionic.wang
ionic china http://ionic-china.com/index.html
