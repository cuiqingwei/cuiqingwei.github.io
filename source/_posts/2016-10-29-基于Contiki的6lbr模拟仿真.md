---
layout: post
title: '基于Contiki的6lbr模拟仿真'
date: 2016-10-29 21:04:40
comments: true
tags:
  - Contiki
  - Cooja
  - 6lbr
---

# Instant Contiki 2.7

下载、安装... 具体请查看链接->[http://www.contiki-os.org/start.html](http://www.contiki-os.org/start.html)

# Java版本

打开（_Ctrl+Alt+T_）超级终端，键入 **java -version**

```
user@instant-contiki:~$ java -version
java version "1.7.0_25"
OpenJDK Runtime Environment (IcedTea 2.3.10) (7u25-2.3.10-1ubuntu0.12.04.2)
OpenJDK Client VM (build 23.7-b01, mixed mode, sharing)
```

# 安装网桥管理工具 **bridge-utils** 包

```
sudo apt-get install bridge-utils
```

<!--more-->

# 获取[6lbr](http://cetic.github.com/6lbr)代码，初始化子模块

```
git clone --recursive https://github.com/cetic/6lbr
cd 6lbr/examples/6lbr
```

# 在6lbr源目录，执行以下命令：

```
make TARGET=native all
make tools
```

# 启动一个 **1-BR COOJA** 实例

<font style="font-weight:bold;"color="red">警告：</font> 默认Instant Contiki 2.7环境中没有添加serial2pty插件，添加方法是：

- 运行cooja

```
cd ~/6lbr/tools/cooja
ant run
```

Settings -> External tools paths... 添加

**;[CONTIKI_DIR]/tools/cooja/apps/serial2pty**

![](/img/20161029/6lbr1.png)

设置完成后，关闭。运行下面命令，启动实例子

```
cd examples/6lbr/test
make cooja-small
```

![](/img/20161029/6lbr2.png)

运行成功，console会输出如下结尾的信息，详见下图：

```
INFO [main] (Serial2Pty.java:116) - serialpty;open;9;/dev/pts/11
INFO [main] (Serial2Pty.java:116) - serialpty;open;10;/dev/pts/12
INFO [main] (Serial2Pty.java:116) - serialpty;open;11;/dev/pts/13
INFO [main] (Serial2Pty.java:116) - serialpty;open;12;/dev/pts/14
```

![](/img/20161029/6lbr3.png)

通过鼠标右击节点，也可打开其他节点的串口监控，如下图所示：

![](/img/20161029/6lbr4.png)

# 运行一个6lbr实例

Cooja _Simulation control_ 点击 **Start**

```
cd examples/6lbr/test
sudo make launch-6lbr-router-1-cooja
```

运行后，终端键入 **ifconfig** 会发现多了 [br0网桥接口](/2015/07/06/2015-07-06-OpenWRT下ifconfig各接口说明/)

![](/img/20161029/6lbr5.png)

**如果要避免重复输出**

```
Fetching MAC address
Fetching MAC address
```

可以

```
cd ~/6lbr/examples/6lbr/test/conf-templates
vi cooja-1-router.conf
```

最后两行，指定日志写入 **/var/log/6lbr.log**，如下图所示：

![](/img/20161029/6lbr6.png)

# 访问6lbr **webserver** 界面

打开浏览器，地址栏输入 **[bbbb::100]** 回车，看到如下界面就算成功了！

![](/img/20161029/6lbr7.png)
