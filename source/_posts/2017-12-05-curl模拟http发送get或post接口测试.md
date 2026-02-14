---
layout: post
title: 'curl模拟http发送get或post接口测试'
date: 2017-12-05 14:12:08
comments: true
tags:
  - curl
---

# 一、get请求

curl "http://www.baidu.com" 如果这里的URL指向的是一个文件或者一幅图都可以直接下载到本地
curl -i "http://www.baidu.com" 显示全部信息
curl -l "http://www.baidu.com" 只显示头部信息
curl -v "http://www.baidu.com" 显示get请求全过程解析
wget "http://www.baidu.com" 也可以

<!--more-->

# 二、post请求

curl -d "param1=value1&param2=value2" "http://www.baidu.com"

# 三、json格式的post请求

curl -l -H "Content-type: application/json" -X POST -d '{"phone":"13521389587","password":"test"}' http://domain/apis/users.json

> 例如：curl -l -H "Content-type: application/json" -X POST -d '{"ver": "1.0","soa":{"req":"123"},"iface":"me.ele.lpdinfra.prediction.service.PredictionService","method":"restaurant_make_order_time","args":{"arg2":"\"stable\"","arg1":"{\"code\":[\"WIND\"],\"temperature\":11.11}","arg0":"{\"tracking_id\":\"100000000331770936\",\"eleme_order_id\":\"100000000331770936\",\"platform_id\":\"4\",\"restaurant_id\":\"482571\",\"dish_num\":1,\"dish_info\":[{\"entity_id\":142547763,\"quantity\":1,\"category_id\":1,\"dish_name\":\"[0xe7][0x89][0xb9][0xe4][0xbb][0xb7][0xe8][0x85][0x8a][0xe5][0x91][0xb3][0xe5][0x8f][0x89][0xe7][0x83][0xa7][0xe5][0x8f][0x8c][0xe6][0x8b][0xbc][0xe7][0x85][0xb2][0xe4][0xbb][0x94][0xe9][0xa5][0xad]\",\"price\":31.0}],\"merchant_location\":{\"longitude\":\"121.47831425\",\"latitude\":\"31.27576153\"},\"customer_location\":{\"longitude\":\"121.47831425\",\"latitude\":\"31.27576153\"},\"created_at\":1477896550,\"confirmed_at\":1477896550,\"dishes_total_price\":0.0,\"food_boxes_total_price\":2.0,\"delivery_total_price\":2.0,\"pay_amount\":35.0,\"city_id\":\"1\"}"}}' http://vpcb-lpdinfra-stream-1.vm.elenet.me:8989/rpc

ps：json串内层参数需要格式化

# 四、高级功能

curl命令是一个功能强大的网络工具，它能够通过http、ftp等方式下载文件，也能够上传文件。其实curl远不止前面所说的那些功能，大家可以通过man curl阅读手册页获取更多的信息。类似的工具还有wget。

curl命令使用了libcurl库来实现，libcurl库常用在C程序中用来处理HTTP请求，curlpp是libcurl的一个C++封装，这几个东西可以用在抓取网页、网络监控等方面的开发，而curl命令可以帮助来解决开发过程中遇到的问题。

- 1，抓取页面内容到一个文件中

curl -o home.html http://www.baidu.com --将百度首页内容抓下到home.html中
curl -o #2\_#1.jpghttp://cgi2.tky.3web.ne.jp/~{A,B}/[001-201].JPG

由于A/B下的文件名都是001，002...，201，下载下来的文件重名，这样，自定义出来下载下来的文件名，就变成了这样：原来： A/001.JPG —-> 下载后： 001-A.JPG 原来： B/001.JPG ---> 下载后： 001-B.JPG

- 2，用-O（大写的），后面的url要具体到某个文件，不然抓不下来。还可以用正则来抓取东西

curl -O http://img.voidcn.com/vcimg/000/000/767/511_420_fe4.gif

运行结果如下：

% Total % Received % Xferd Average Speed Time Time Time Current
Dload Upload Total Spent Left Speed
100 1575 100 1575 0 0 14940 0 --:--:-- --:--:-- --:--:-- 1538k
会在当前执行目录中生成一张bdlogo.gif的图片。

curl -O http://XXXXX/screen[1-10].JPG --下载screen1.jpg~screen10.jpg

- 3，模拟表单信息，模拟登录，保存cookie信息

curl -c ./cookie_c.txt -F log=aaaa -F pwd=**\*\***http://www.XXXX.com/wp-login.php

- 4，模拟表单信息，模拟登录，保存头信息

curl -D ./cookie_D.txt -F log=aaaa -F pwd=**\*\***http://www.XXXX.com/wp-login.php

-c(小写)产生的cookie和-D里面的cookie是不一样的。

- 5，使用cookie文件

curl -b ./cookie_c.txt http://www.XXXX.com/wp-admin

- 6，断点续传，-C(大写)

curl -C -O http://img.voidcn.com/vcimg/000/000/767/511_420_fe4.gif

- 7，传送数据,最好用登录页面测试，因为你传值过去后，curl回抓数据，你可以看到你传值有没有成功

curl -d log=aaaa http://www.XXXX.com/wp-login.php

- 8，显示抓取错误，下面这个例子，很清楚的表明了。

curl -fhttp://www.XXXX.com/asdf

curl: (22) The requested URL returned error: 404

curlhttp://www.XXXX.com/asdf

<HTML><HEAD><TITLE>404,not found</TITLE>

- 9，伪造来源地址，有的网站会判断，请求来源地址，防止盗链。

curl -ehttp://localhosthttp://www.XXXX.com/wp-login.php

- 10，当我们经常用curl去搞人家东西的时候，人家会把你的IP给屏蔽掉的,这个时候,我们可以用代理

curl -x 24.10.28.84:32779 -o home.htmlhttp://www.XXXX.com

- 11，比较大的东西，我们可以分段下载

curl -r 0-100 -o img.part1http://www.XXXX.com/wp-content/uploads/2010/09/compare_varnish.jpg

% Total % Received % Xferd Average Speed Time Time Time Current
Dload Upload Total Spent Left Speed
100 101 100 101 0 0 105 0 --:--:-- --:--:-- --:--:-- 0
curl -r 100-200 -o img.part2http://www.XXXX.com/wp-ontent/uploads/2010/09/compare_varnish.jpg
% Total % Received % Xferd Average Speed Time Time Time Current
Dload Upload Total Spent Left Speed
100 101 100 101 0 0 57 0 0:00:01 0:00:01 --:--:-- 0
curl -r 200- -o img.part3http://www.XXXX.com/wp-content/uploads/2010/09/compare_varnish.jpg
% Total % Received % Xferd Average Speed Time Time Time Current
Dload Upload Total Spent Left Speed
100 104k 100 104k 0 0 52793 0 0:00:02 0:00:02 --:--:-- 88961

ls |grep part | xargs du -sh

4.0K one.part1
112K three.part3
4.0K two.part2

用的时候，把他们cat一下就OK,cat img.part\* >img.jpg

- 12，不会显示下载进度信息

curl -s -o aaa.jpg http://img.voidcn.com/vcimg/000/000/767/511_420_fe4.gif

- 13，显示下载进度条

curl -0 http://img.voidcn.com/vcimg/000/000/767/511_420_fe4.gif (以http1.0协议请求)

####################################################################### 100.0%

- 14,通过ftp下载文件

curl -u用户名:密码 -Ohttp://www.XXXX.com/demo/curtain/bbstudy_files/style.css

% Total % Received % Xferd Average Speed Time Time Time Current
Dload Upload Total Spent Left Speed
101 1934 101 1934 0 0 3184 0 --:--:-- --:--:-- --:--:-- 7136

curl -u 用户名:密码 -O http://www.XXXX.com/demo/curtain/bbstudy_files/style.css

% Total % Received % Xferd Average Speed Time Time Time Current
Dload Upload Total Spent Left Speed
101 1934 101 1934 0 0 3184 0 --:--:-- --:--:-- --:--:-- 7136

或者用下面的方式

curl -O ftp://用户名:密码@ip:port/demo/curtain/bbstudy_files/style.css
curl -O ftp://用户名:密码@ip:port/demo/curtain/bbstudy_files/style.css

- 15，通过ftp上传

curl -T test.sql ftp://用户名:密码@ip:port/demo/curtain/bbstudy_files/
curl -T test.sql ftp://用户名:密码@ip:port/demo/curtain/bbstudy_files/

- 15,模拟浏览器头

curl -A "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0)" -x 123.45.67.89:1080 -o page.html -D cookie0001.txthttp://www.www.baidu.com

- 16,PUT、GET、POST

比如 curl -T localfile http://cgi2.tky.3web.ne.jp/~zz/abc.cgi，这时候，使用的协议是HTTP的PUT method
刚才说到PUT，自然想起来了其他几种methos－－GET和POST。
http提交一个表单，比较常用的是POST模式和GET模式
GET模式什么option都不用，只需要把变量写在url里面就可以了
比如：
curl http://www.yahoo.com/login.cgi?user=nick&password=12345
而POST模式的option则是 -d
比如，curl -d "user=nick&password=12345" http://www.yahoo.com/login.cgi
就相当于向这个站点发出一次登陆申请~~~~~
到底该用GET模式还是POST模式，要看对面服务器的程序设定。
一点需要注意的是，POST模式下的文件上的文件上传，比如

```
<form method="POST" enctype="multipar/form-data" action="http://cgi2.tky.3web.ne.jp/~zz/up_file.cgi">
<input type=file name=upload>
<input type=submit name=nick value="go">
</form>
```

这样一个HTTP表单，我们要用curl进行模拟，就该是这样的语法：

```
curl -F upload=@localfile -F nick=go http://cgi2.tky.3web.ne.jp/~zz/up_file.cgi
```
