---
layout: post
title: 'GitHub中的watch、star、fork的作用'
date: 2016-12-15 21:04:40
comments: true
tags:
  - GitHub
  - GoAhead
---

> 转自：https://www.cnblogs.com/bibi-feiniaoyuan/p/9519467.html

在每个 github 项目的右上角，都有三个按钮,分别是 watch、star、fork，但是有些刚开始使用 github 的同学，可能对这三个按钮的使用却不怎么了解，包括一开始使用 github 的我也是如此，这篇博客，结合自己的理解和使用，说说这三个按钮的用法以及一些个人见解。

如下图所示这是我们经常看到的三个按钮。

![img](https://images2018.cnblogs.com/blog/994461/201808/994461-20180822180828688-21227169.png)

从左至右，依次是 watch star fork,下面分别说下他们的具体作用。

<!--more--->

## **watch**

watch翻译过来可以称之为观察，点击watch可以看到如下的列表。

![img](https://images2018.cnblogs.com/blog/994461/201808/994461-20180822180842356-821161803.png)

默认每一个用户都是处于Not watching的状态，当你选择Watching，表示你以后会关注这个项目的所有动态，以后只要这个项目发生变动，如被别人提交了pull request、被别人发起了issue等等情况，

你都会在自己的个人通知中心，收到一条通知消息，如果你设置了个人邮箱，那么你的邮箱也可能收到相应的邮件

如下，我 watch 了开源项目[android-cn/android-discuss](https://github.com/android-cn/android-discuss)，那么以后任何人只要在这个项目下提交了 issue 或者在 issue 下面有任何留言，

我的通知中心就会通知我。如果你配置了邮箱，你还可能会因此不断的收到邮件。

![img](https://images2018.cnblogs.com/blog/994461/201808/994461-20180822180856110-429138577.png)

如果你不想接受这些通知，那么点击 Not Watching 即可。

另外这里有一篇文章讲 [如何正确接收 GitHub 的消息邮件](https://github.com/cssmagic/blog/issues/49)，很不错的一篇文章，推荐大家看看。

## star

star 翻译过来应该是星星，但是这个翻译没任何具体意义，这里解释为`关注`或者`点赞`更合适，当你点击 star,表示你喜欢这个项目或者通俗点，可以把他理解成朋友圈的点赞吧，表示对这个项目的支持。

不过相比朋友圈的点赞，github 里面会有一个列表，专门收集了你所有 start 过的项目，

点击 github 个人头像，可以看到 your star的条目，点击就可以查看你 star 过的所有项目了。如下图

![img](https://images2018.cnblogs.com/blog/994461/201808/994461-20180822180927593-1376654966.png)

不过，在你的 star 列表很容易出现这样的问题。就是你可能 star 成百上千个项目怎么办。

这时，如果 github 可以提供一个分类功能该多好，就像微博网页版的收藏，你在收藏的时候可以设置 tag，

这样设置的好处是，以后再次查找项目时，可以根据归类查找，但是不知道 github 的产品经理是怎么想的，

github 本身没有这个功能，但是 github 从来也不缺有思想有执行力的程序员，

这不，前段时间就有人做了一个 Chrome 插件，这个插件可以对 github 中所有 star 的项目进行分类，

如下所示，注意看图片右侧，多了一个 Filter by tag 列表。

![img](https://images2018.cnblogs.com/blog/994461/201808/994461-20180822180956857-1025289775.png)

[下载地址.](https://chrome.google.com/webstore/detail/github-stars-tagger/aaihhjepepgajmehjdmfkofegfddcabc)

尽管这个插件已经很好了，但是还是有缺点，你只能 star完 项目了，去 star 列表后，才能对项目打 tag，这是很不方便的。

真心希望，未来 github 可以自己支持对 star 的 tag 处理。憧憬...

#

#### **更新**

也许是 github 自己也发现了检索的问题，现在 github 增加了一个类似的功能，可以给自己给自己的项目设置 topic. 如下图所示。

![img](https://upload-images.jianshu.io/upload_images/588640-9fc3057dca0c0e4e.png)

topic

给自己的项目设置 topic 后，相当于自己给自己的项目设置了一个 tag ，这样可以方便别人搜索。比如要搜索所有 topic 为 android 的项目，你只需要在 GitHub 搜索时输入 `topic android ` 然后搜索即可。

# fork

当选择 fork，相当于你自己有了一份原项目的拷贝，当然这个拷贝只是针对当时的项目文件，如果后续原项目文件发生改变，你必须通过其他的方式去同步。

一般来说，我们不需要使用 fork 这个功能，除非有一些项目，可能存在 bug 或者可以继续优化的地方，你想帮助原项目作者去完善这个项目或者单纯的想在原来项目基础上己维护一个属于自己项目（比如我 fork 的 [AndroidWeekly 客户端](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fmaoruibin%2FAndroidWeekly)，那么你可以 fork 一份项目下来，然后自己对这个项目进行修改完善，当你觉得项目没问题了，你就可以尝试发起 pull request 给原项目作者了。

然后就静静等待他的 merge 邮件通知了。

我看到很多人错误的在使用 fork。很多人把 fork 当成了收藏一样的功能，包括一开始使用 github 的我，每次看到一个好的项目就先 fork，

因为这样，就可以我的 repository(仓库)列表下查看 fork 的项目了。其实你完全可以使用 star 来达到这个目的。

## 使用建议

1、对于一些可能会经常发生变化的会不定期更新的好项目 多使用 watch.

比如 android-cn 团队的 [android-discuss](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fandroid-cn%2Fandroid-discuss) 项目，

你就可以 watching 它，这里面都是一些关于 Android 技术的交流，如果有任何新问题，你都可以收到通知，你可以查看别人的回答，

你可以回答别人提出的问题，这是一个很好的学习成长方式。

其他值得watch的项目还有很多，比如 github 上很多的 Awesome 系列的项目,如 [Awesome-MaterialDesign](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2FlightSky%2FAwesome-MaterialDesign) 等，你 watch 这些项目了，

只要项目新增一些好玩好用的东西，你就会收到通知。

我在知乎上看到有人问这样的问题，说 github 上有哪些值得 watch 的项目，其实有很多，我自己也整理了一些，但是没放到 github.

值得注意的是，如果 watch多了，你可能会被无休止的邮件通知烦死（邮件通知可设置），因为被 watch 项目有任何留言、PR等更新都会触发通知，所以做好权衡。

2、喜欢一个项目就 star 它吧~

3、修改开源项目就使用 fork，这样你就可以在原项目的基础上，对项目进行修改提交，现在你是这个项目的主人啦~

## 小细节

有些时候，你看到一个项目的 star 数有很多，你就想知道到底都有那些人 star 了这个项目，或者 fork 了这个项目，

但是环顾一圈，你却找不到一个入口，后来自己不经意的发现，只要`点击 star 傍边的数字`，就可以查看有哪些人 star 了这个项目。

是不是有点意思，现在你就可以去试试，watch、fork上面的数字都是可以点击的，道理一样。
