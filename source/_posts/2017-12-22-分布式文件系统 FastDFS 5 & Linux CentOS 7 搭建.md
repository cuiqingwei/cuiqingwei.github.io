---
layout: post
title: '分布式文件系统 FastDFS 5 & Linux CentOS 7 搭建'
date: 2017-12-05 14:12:08
comments: true
tags:
  - FastDFS
---

# 简介

FastDFS是一个开源的轻量级分布式文件系统，由跟踪服务器（tracker server）、存储服务器（storage server）和客户端（client）三个部分组成，主要解决了海量数据存储问题，特别适合以中小文件（建议范围：**4KB < file_size <500MB** ）为载体的在线服务。

<!--more-->

FastDFS的系统结构图如下：

![](/img/20171222/20160922105102336.jpg)

如上图，FastDFS的两个核心概念分别是：

- Tracker（跟踪器）
- Storage（存储节点）

> Tracker主要做调度工作，相当于mvc中的controller的角色，在访问上起负载均衡的作用。跟踪器和存储节点都可以由一台或多台服务器构成，跟踪器和存储节点中的服务器均可以随时增加或下线而不会影响线上服务，其中跟踪器中的所有服务器都是对等的，可以根据服务器的压力情况随时增加或减少。Tracker负责管理所有的Storage和group，每个storage在启动后会连接Tracker，告知自己所属的group等信息，并保持周期性的心跳，tracker根据storage的心跳信息，建立group==>[storage server list]的映射表，Tracker需要管理的元信息很少，会全部存储在内存中；另外tracker上的元信息都是由storage汇报的信息生成的，本身不需要持久化任何数据，这样使得tracker非常容易扩展，直接增加tracker机器即可扩展为tracker cluster来服务，cluster里每个tracker之间是完全对等的，所有的tracker都接受stroage的心跳信息，生成元数据信息来提供读写服务。

> Storage采用了分卷[Volume]（或分组[group]）的组织方式，存储系统由一个或多个组组成，组与组之间的文件是相互独立的，所有组的文件容量累加就是整个存储系统中的文件容量。一个卷[Volume]（组[group]）可以由一台或多台存储服务器组成，一个组中的存储服务器中的文件都是相同的，组中的多台存储服务器起到了冗余备份和负载均衡的作用，数据互为备份，存储空间以group内容量最小的storage为准，所以建议group内的多个storage尽量配置相同，以免造成存储空间的浪费。更多原理性的内容可以参考这篇blog，介绍的很详细：[分布式文件系统FastDFS设计原理](http://blog.chinaunix.net/uid-20196318-id-4058561.html)

Parallels安装Centos7

![](/img/20171222/WX20171222-173110@2x.png)

# 下载

```
wget https://github.com/happyfish100/fastdfs/archive/V5.11.tar.gz
wget https://github.com/happyfish100/libfastcommon/archive/V1.0.36.tar.gz
```

![](/img/20171222/WX20171222-143233@2x.png)

如上图，由于FastDFS是纯C语言实现，只支持Linux、FreeBSD等UNIX系统，所以我们直接下载tar.gz的压缩包，同时FastDFS 5.11同以前版本相比将公共的一些函数等单独封装成了libfastcommon包，所以在安装FastDFS之前我们还必须安装libfastcommon，在余的GitHub首页可以看到：

![](/img/20171222/20160922114506577.png)

下载完成后，就可以开始解压安装了：

![](/img/20171222/WX20171222-143631@2x.png)

# 安装

先要安装gcc编译器：

```
yum -y install gcc-c++
```

## libfastcommon

```
tar -zxvf V1.0.36.tar.gz
cd libfastcommon-1.0.36
./make.sh
./make.sh install
```

## FastDFS

```
tar -zxvf V5.11.tar.gz
cd fastdfs-5.11
./make.sh
./make.sh install
```

## 配置文件

安装成功后就会生成如上的3个.sample文件（示例配置文件），拷贝出3个后面用的配置文件：

```
cd /etc/fdfs
cp client.conf.sample client.conf
cp storage.conf.sample storage.conf
cp tracker.conf.sample tracker.conf
```

至此FastDFS已经安装完毕，接下来的工作就是依次配置Tracker和Storage了。

# Tracker

在配置Tracker之前，首先需要创建Tracker服务器的文件路径，即用于存储Tracker的数据文件和日志文件等，我这里选择在 **/opt** 目录下创建一个 **fastdfs_tracker** 目录用于存放Tracker服务器的相关文件：

```
mkdir /opt/fastdfs_tracker
```

接下来就要重新编辑上一步准备好的 **/etc/fdfs** 目录下的 **tracker.conf** 配置文件，打开文件后依次做以下修改：

> disabled=false #启用配置文件（默认启用）
> port=22122 #设置tracker的端口号，通常采用22122这个默认端口
> base_path=/opt/fastdfs_tracker #设置tracker的数据文件和日志目录
> http.server_port=6666 #设置http端口号，默认为8080

配置完成后就可以启动Tracker服务器了，但首先依然要为启动脚本创建软引用，因为fdfs_trackerd等命令在/usr/local/bin中并没有，而是在/usr/bin路径下：

```
ln -s /usr/bin/fdfs_trackerd /usr/local/bin
ln -s /usr/bin/stop.sh /usr/local/bin
ln -s /usr/bin/restart.sh /usr/local/bin
```

最后通过命令启动Tracker服务器：

```
service fdfs_trackerd start
```

如果启动命令执行成功，那么同时在刚才创建的tracker文件目录 **/opt/fastdfs_tracker** 中就可以看到启动后新生成的data和logs目录，tracker服务的端口也应当被正常监听，最后再通过netstat命令查看一下端口监听情况.

```
netstat -unltp|grep fdfs
```

Tracker至此就配置好了，接下来就可以配置FastDFS的另一核心——Storage。

# Storage

同理，步骤基本与配置Tracker一致，首先是创建Storage服务器的文件目录，需要注意的是同Tracker相比我多建了一个目录，因为Storage还需要一个文件存储路径，用于存放接收的文件：

```
mkdir /opt/fastdfs_storage
mkdir /opt/fastdfs_storage_data
```

接下来修改/etc/fdfs目录下的storage.conf配置文件，打开文件后依次做以下修改：

> disabled=false #启用配置文件（默认启用）
> group_name=group1 #组名，根据实际情况修改
> port=23000 #设置storage的端口号，默认是23000，同一个组的storage端口号必须一致
> base_path=/opt/fastdfs_storage #设置storage数据文件和日志目录
> store_path_count=1 #存储路径个数，需要和store_path个数匹配
> store_path0=/opt/fastdfs_storage_data #实际文件存储路径
> tracker_server=10.211.55.8:22122 #tracker 服务器的 IP地址和端口号，如果是单机搭建，IP不要写127.0.0.1，否则启动不成功（此处的ip是我的CentOS虚拟机ip）
> http.server_port=8888 #设置 http 端口号

配置完成后同样要为Storage服务器的启动脚本设置软引用：

```
ln -s /usr/bin/fdfs_storaged /usr/local/bin
```

接下来就可以启动Storage服务了：

```
service fdfs_storaged start
```

同理，如果启动成功，/opt/fastdfs_storage中就可以看到启动后新生成的 **data** 和 **logs** 目录，端口23000也应被正常监听，还有一点就是文件存储路径下会生成多级存储目录，那么接下来看看是否启动成功.

```
netstat -unltp|grep fdfs
```

![](/img/20171222/WX20171222-162905@2x.png)

如上图，可以看到此时已经正常监听tracker的22122端口和storage的23000端口，至此storage服务器就已经配置完成，确定了storage服务器启动成功后，还有一项工作就是看看storage服务器是否已经登记到 tracker服务器（也可以理解为tracker与storage是否整合成功），运行以下命令：

```
/usr/bin/fdfs_monitor /etc/fdfs/storage.conf
```

![](/img/20171222/WX20171222-163212@2x.png)

如上所示，看到 **ip_addr = 10.211.55.8 (centos-7.shared) ACTIVE** 字样即可说明storage服务器已经成功登记到了tracker服务器.

添加开机启动，检查一下rc.local是否具备可执行权限，若是无可执行权限则通过

```
chmod +x /etc/rc.d/rc.local
```

进行授权，如下图：

![](/img/20171222/WX20171222-163634@2x.png)

至此我们就已经完成了fastdfs的全部配置，此时也就可以用客户端工具进行文件上传下载的测试了。

# 上传测试

测试时需要设置客户端的配置文件，编辑 **/etc/fdfs** 目录下的 **client.conf** 文件，打开文件后依次做以下修改：

> base_path=/opt/fastdfs_tracker #tracker服务器文件路径
> tracker_server=10.211.55.8:22122 #tracker服务器IP地址和端口号
> http.tracker_server_port=6666 #tracker服务器的http端口号，必须和tracker的设置对应起来

配置完成后就可以模拟文件上传了，先通过scp向 **/tmp** 目录下传一下文件。

然后通过执行客户端上传命令尝试上传：

```
/usr/bin/fdfs_upload_file /etc/fdfs/client.conf /tmp/big.jpg
```

运行后可以发现给我们返回了一个路径：

![](/img/20171222/WX20171222-164154@2x.png)

这就表示我们的文件已经上传成功了，当文件存储到某个子目录后，即认为该文件存储成功，接下来会为该文件生成一个文件名，文件名由group、存储目录、两级子目录、fileid、文件后缀名（由客户端指定，主要用于区分文件类型）拼接而成，如下图：

![](/img/20171222/20160923132829236.jpg)

同时在之前配置的storage服务器的实际文件存储路径中也可以根据返回的路径找到实际文件：

# HTTP访问文件

因为FastDFS目前已不支持http协议，**4.0.5** 版本开始移除了自带的HTTP支持（因为之前自带的HTTP服务较为简单，无法提供负载均衡等高性能服务），所以余大提供了nginx上使用FastDFS的模块fastdfs-nginx-module，下载地址如下：[https://github.com/happyfish100/fastdfs-nginx-module](https://github.com/happyfish100/fastdfs-nginx-module)，这样做最大的好处就是提供了HTTP服务并且解决了group中storage服务器的同步延迟问题，接下是fastdfs-nginx-module的安装配置过程.

## fastdfs-nginx-module

先安装系统依赖

```
yum -y install pcre pcre-devel zlib zlib-devel openssl openssl-devel git

```

下载所需组件

```
cd /usr/local/src/
git clone https://github.com/happyfish100/fastdfs-nginx-module.git
wget http://nginx.org/download/nginx-1.8.1.tar.gz
```

## storage nginx

首先是为storage服务器安装nginx

```
tar -zxvf nginx-1.8.1.tar.gz
cd nginx-1.8.1
./configure --prefix=/usr/local/nginx --add-module=/usr/local/src/fastdfs-nginx-module/src
make && make install
```

接下来要修改一下nginx的配置文件，进入conf目录并打开 **nginx.conf** 修改，如下图：

```
vi /usr/local/nginx/conf/nginx.conf
```

![](/img/20171222/WX20171222-165331@2x.png)

然后进入FastDFS的安装目录 **/usr/local/src/fastdfs-5.11** 目录下的conf目录，将http.conf和mime.types拷贝到 **/etc/fdfs** 目录下：

```
cp -r /usr/local/src/fastdfs-5.11/conf/http.conf /etc/fdfs/
cp -r /usr/local/src/fastdfs-5.11/conf/mime.types /etc/fdfs/
```

接下来还需要把fastdfs-nginx-module安装目录中src目录下的mod_fastdfs.conf也拷贝到 **/etc/fdfs** 目录下：

```
cp -r /usr/local/src/fastdfs-nginx-module/src/mod_fastdfs.conf /etc/fdfs/
```

就需要编辑刚拷贝的这个mod_fastdfs.conf文件了，打开mod_fastdfs.conf并按顺序依次编译以下内容：

> base_path=/opt/fastdfs_storage #保存日志目录
> tracker_server=10.211.55.8:22122 #tracker服务器的IP地址以及端口号
> storage_server_port=23000 #storage服务器的端口号
> url_have_group_name = true #文件 url 中是否有 group 名
> store_path0=/opt/fastdfs_storage_data #存储路径
> group_count = 3 #设置组的个数，事实上这次只使用了group1

设置了group_count = 3，接下来就需要在文件尾部追加这3个group setting：

> [group1]
> group_name=group1
> storage_server_port=23000
> store_path_count=1
> store_path0=/opt/fastdfs_storage_data

> [group2]
> group_name=group2
> storage_server_port=23000
> store_path_count=1
> store_path0=/opt/fastdfs_storage_data

> [group3]
> group_name=group3
> storage_server_port=23000
> store_path_count=1
> store_path0=/opt/fastdfs_storage_data

接下来还需要建立 M00 至存储目录的符号连接：

```
ln  -s  /opt/fastdfs_storage_data/data  /opt/fastdfs_storage_data/data/M00
```

最后启动nginx：

```
/usr/local/nginx/sbin/nginx
```

通过浏览器访问 http://10.211.55.8:9999/ 也可以看到nginx的主页：

## tracker nginx

同理，再装一个nginx，目录命名为 **nginx2** ，安装路径依旧放在 **/usr/local** 下，由于和之前一样，此处就不再做详细解释：

```
cd /usr/loca/src/nginx-1.8.1
./configure --prefix=/usr/local/nginx2 --add-module=/usr/local/src/fastdfs-nginx-module/src
make && make install
```

接下来依然是修改nginx2的配置文件，进入conf目录并打开nginx.conf文件加入以下配置，storage的nginx无需修改listen端口，即默认的80端口，并将upstream指向tracker的nginx地址：

```
vi /usr/local/nginx2/conf/nginx.conf
```

![](/img/20171222/WX20171222-170041@2x.png)

至此关于fastdfs就已经全部配置完毕了，再一次进行测试看看是否能正常上传文件并通过http访问文件。

上传文件

```
scp big.jpg root@10.211.55.8:/tmp/
cd /tmp/
/usr/bin/fdfs_upload_file /etc/fdfs/client.conf big.jpg
/usr/bin/fdfs_upload_file /etc/fdfs/client.conf CADENCE+SPB+15.7工程实例入门.pdf
/usr/bin/fdfs_upload_file /etc/fdfs/client.conf Step\ by\ Step_\ How\ to\ easily\ tie\ the\ FG\ Knot.mp4
```

接下来的关键就是通过HTTP测试文件访问，打开浏览器输入ip地址+文件名看看是否能正常访问该图片：

http://10.211.55.8:9999/group1/M00/00/00/CtM3CFo8ysuALXURAAyIkvltU0w661.jpg
http://10.211.55.8:9999/group1/M00/00/00/CtM3CFo8ytGABqkAAnhI9lHEHhw379.pdf
http://10.211.55.8:9999/group1/M00/00/00/CtM3CFo8ytaAZqEqA8pGTvhXcBQ424.mp4

![](/img/20171222/WX20171222-170825@2x.png)

一切正常~ 至此关于FastDFS在CentOS 7下的部署测试就已经全部完成。
