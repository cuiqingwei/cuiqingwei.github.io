---
layout: post
title: '在Mac上运行TensorFlow的PoseNet'
date: 2020-02-16 21:37
comments: true
tags:
  - TensorFlow
  - 人工智能
---

项目地址：https://github.com/tensorflow/tfjs-models

首先将项目clone下来

```sh
git clone https://github.com/tensorflow/tfjs-models.git
```

<!--more-->

进入PoseNet目录

```shell
cd posenet/
```

使用yarn安装，如果电脑上没有安装yarn，可以先使用homebrew安装yarn

```shell
brew install yarn
```

运行安装

```shell
yarn
```

进入demos文件夹

```shell
cd demos
```

安装依赖关系并准备build目录：

```js
yarn;
```

安装完成，现在可以运行demo了

```js
yarn watch
```

此时弹出一个浏览器窗口

点击Coco images demo则会演示如何在图像中进行姿态估计，并且演示了多人和单人姿态估计之间的差异

![multi-person](https://miro.medium.com/max/513/1*EZOqbMLkIwBgyxrKLuQTHA.png)

点击Camera feed demo会打开摄像头，进行实时的姿态估计

![one-person](https://miro.medium.com/max/600/1*BKZqEPtvM-6xwarhABZQnA.gif)
