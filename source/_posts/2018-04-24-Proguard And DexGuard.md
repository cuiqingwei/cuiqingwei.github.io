---
layout: post
title: 'Proguard And DexGuard'
date: 2018-04-24 12:12:08
comments: true
tags:
  - android
---

一般情况下，我们混淆自己的安卓项目，会使用Proguard。

http://www.saikoa.com/proguard

**Progurad** 是免费的，而且已经集成到Android ADT中了，使用起来很方便。

混淆工作，是为了保护我们的代码。但是，即使使用Pgoruard混淆了我们的apk文件，也可以使用apktool工具反编译出资源，能够看到Manifest文件，res资源等等。也可以使用dex2jar工具生成jar文件，进而使用jd工具反编译出Java代码片段。虽然，这些代码大部分都经过了混淆，已经很难阅读，但是还是可以读的。

<!--more-->

更危险的是，Proguard只能保护代码，却不能保护我们的apk文件。任何人都可以使用apktool工具，反编译我们开发的apk文件，进而更改其中各种资源，或者更改部分代码，甚至是注入代码，然后再打包回apk，二次发布后，达到自己的目的。或者是加入了广告，或者是增加了恶意木马病毒，等等。

还好，我们还有一种选择，DexGuard。

http://www.saikoa.com/dexguard

**DexGuard** 是收费的，虽然挺贵的，但是对于一个公司来说，还是可以承受的。

DexGuard是在Proguard基础上，加入了更多的保护措施。使用DexGuard混淆后，生成的apk文件，就无法正常使用apktool反编译了。尽管还是能够反编译出部分资源文件，但是由于反编译过程不完全，就无法再打包成apk了。这样就保护了我们的apk文件，不会被二次打包发布了。
