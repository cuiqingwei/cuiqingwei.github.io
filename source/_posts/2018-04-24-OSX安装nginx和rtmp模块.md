---
layout: post
title: 'OSX安装nginx和rtmp模块'
date: 2018-04-24 12:12:08
comments: true
tags:
  - rtmp
  - nginx
---

# 1.安装brew

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
如果安装后, 想要卸载
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

<!--more-->

# 2.安装nginx

先clone nginx项目到本地

```
brew tap denji/nginx
```

> brew tap homebrew/nginx 源已失效
> 可参考 https://github.com/denji/homebrew-nginx

执行安装：

```
brew install nginx-full --with-rtmp-module
```

此时, nginx和rtmp模块就安装好了
输入命令:

```
nginx
```

在浏览器里打开http://localhost:8080

> 查询安装路径：brew info nginx-full

# 3.配置RTMP服务

/usr/local/etc/nginx/nginx.conf **中RTMP和Http节点一致**

```
nginx -c /usr/local/etc/nginx/nginx.conf
```

> 使用nginx -c的参数指定nginx.conf文件的位置

在http节点后面加上rtmp配置：

```
rtmp {
    server {
        listen 1935;
        application myapp {
            live on;
            #record keyframes;
            #record_path /tmp;
            #record_max_size 128K;
            #record_interval 30s;
            #record_suffix .this.is.flv;
            #on_publish http://localhost:8080/publish;
            #on_play http://localhost:8080/play;
            #on_record_done http://localhost:8080/record_done;
       }
       application hls {
             live on;
             hls on;
             hls_path /tmp/app;
             hls_fragment 5s;
       }
    }
}
```

# 4.查询rtmp服务是否正常

修改完配置文件之后执行

```
nginx -s reload
```

执行

```
sudo lsof -i -P | grep -i "listen"
```

查询1935端口是否开启

# 5.直播测试　　

安装ffmpeg

```
brew install ffmpeg
```

安装[vlc播放器](http://www.videolan.org/),准备mp4文件推流测试，用vlc播放

**推流**：

```
ffmpeg -re -i /Volumes/WORK/Moorgen/演示资料/演示视频/moorgen.mp4 -vcodec libx264 -acodec aac -f flv rtmp://127.0.0.1:1935/myapp/room
```

**播放**：

```
rtmp://localhost:1935/myapp/room
```

![](/img/20180424/rtmp.png)
