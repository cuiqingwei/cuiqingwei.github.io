# cuiqingwei.github.io
Gary's Website

## 通过GitHub Pages建立个人站点

> Jekyll-简单的博客、静态网站工具 <http://jekyll.bootcss.com/>

### 1.Git简介
* Git是一个开源的分布式版本控制系统，用以有效、高速的处理从很小到非常大的项目版本管理。
* GitHub可以托管各种git库的站点。
* GitHub Pages免费的静态站点，三个特点：免费托管、自带主题、支持自制页面和Jekyll。

### 2.为什么使用Github Pages
* 搭建简单而且免费；
* 支持静态脚本；
* 可以绑定你的域名；
* DIY自由发挥，动手实践一些有意思的东西git,markdown,bootstrap,jekyll；
* 理想写博环境，git+github+markdown+jekyll；

### 3 创建Github Pages	

**3.1 安装git工具**

> windows <http://windows.github.com/> 

> mac os x <http://mac.github.com/>

**3.2 两种pages模式**

①. User/Organization Pages 个人或公司站点

	1) 使用自己的用户名，每个用户名下面只能建立一个；
	2) 资源命名必须符合这样的规则username/username.github.com；
	3) 主干上内容被用来构建和发布页面

②. Project Pages 项目站点

1) gh-pages分支用于构建和发布；

2) 如果user/org pages使用了独立域名，那么托管在账户下的所有project pages将使用相同的域名进行重定向，除非project pages使用了自己的独立域名；

3) 如果没有使用独立域名，project pages将通过子路径的形式提供服务username.github.com/projectname；

4) 自定义404页面只能在独立域名下使用，否则会使用User Pages 404；

5) 创建项目站点步骤：
	
	$ git clone https://github.com/USERNAME/PROJECT.git PROJECT
	$ git checkout --orphan gh-pages
	$ git rm -rf .
	$ git add .
	$ git commit -a -m "First pages commit"
	$ git push origin gh-pages
	
③. 可以通过User/Organization Pages建立主站，而通过Project Pages挂载二级应用页面。

### 4 使用Jekyll搭建博客

**4.1 什么是jekyll**

Jekyll是一种简单的、适用于博客的、静态网站生成引擎。它使用一个模板目录作为网站布局的基础框架，支持Markdown、Textile等标记语言的解析，提供了模板、变量、插件等功能，最终生成一个完整的静态Web站点。说白了就是，只要安装Jekyll的规范和结构，不用写html，就可以生成网站。[jekyll介绍][jekyll on github][jekyllbootstrap]。

Jekyll使用Liquid模板语言，{{page.title}}表示文章标题，{{content}}表示文章内容。我们可以用两种Liquid标记语言：输出标记（output markup）和标签标记 (tag markup)。输出标记会输出文本（如果被引用的变量存在），而标签标记不会。输出标记是用双花括号分隔，而标签标记是用花括号-百分号对分隔。[Liquid模板语言] [Liquid模板变量参考]。

jekyll与github的关系：GitHub Pages一个由 GitHub 提供的用于托管项目主页或博客的服务，jekyll是后台所运行的引擎。

**4.2 jekyll本地环境搭建**

1. 下载最新的RubyInstaller并安装(我下载的是rubyinstaller-1.9.3-p194.exe)，设置环境变量，path中配置C:\Ruby193\bin目录，然后在命令行终端下输入gem update --system来升级gem；

2. 下载最新的DevKit，DevKit是windows平台下编译和使用本地C/C++扩展包的工具。它就是用来模拟Linux平台下的make,gcc,sh来进行编译。但是这个方法目前仅支持通过RubyInstaller安装的Ruby，并双击运行解压到C:\DevKit。然后打开终端cmd，输入下列命令进行安装：

	```
	cd C:\DevKit
	ruby dk.rb init
	ruby dk.rb install
	```
3. 完成上面的准备就可以安装Jekyll了,因为Jekyll是用Ruby编写的,最好的安装方式是通过RubyGems(gem):
	
	```
	gem install Jekyll
	```

	并使用命令检验是否安装成功

	```
	jekyll --version
	```

4. 安装Rdiscount，这个用来解析Markdown标记的包，使用如下命令：

	```
	gem install rdiscount
	```

5. 运行本地工程：

	cd 到工程目录，启动服务：
	
	```
	jekyll --server
	```
	
**4.3 jekyll目录结构**

**_posts：** _posts中的数据文档，通过注入_layouts定义的模板，通过jekyll --server最终生成的静态页面在_sites目录。目录是用来存放你的文章的，一般以日期的形式书写标题。
 _layouts：_layouts中的模板一般指向了_includes/themes中的模板。目录是用来存放模板的，在这里你可以定义页面中不同的头部和底部。

 **_includes：**
 
1) _includes/JB中有一些常用的工具，用于列表显示、评论等；

2) _includes/themes中可参看主题的相关html文档。

3) _includes/themes中的主题一般包含default.html、post.html和page.html三个文档。default.html定义了网站的最上层框架（模板），post.html和page.html是其子框架（模板）。

4) 生成好的html子页面通过default.html的{{ content }}变量调用，生成整个页面。

 **assets** 渲染页面的CSS和JS文档在assets/themes中
 
 **_config.yml** 站点生成需要用到_config.yml配置文件，站点的全局变量在_config.yml中定义，用site.访问；页面的变量在YAML Front Matter中定义，用page.访问，更多的模板变量可参考模板数据。
 
 index.html是你的页面首页。
 
**4.4 Jekyll-Bootstrap创建博客**

①. 创建个人站点，即创建一个新资源，格式为username.github.com；

②. 安装Jekyll-Bootstrap：

	$ git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
	$ cd USERNAME.github.com
	$ git remote set-url origin git@github.com:USERNAME/USERNAME.github.com.git
	$ git push origin master

③. 访问创建好的个人站点：username.github.com

④. 在本地测试查看效果：	
	```
	cd USERNAME.github.com
	jekyll --server
	```
	
**4.5 Jekyll 写博过程**

①、 配置_config.yml：

1) 修改标题：title : My Blog =)

2) 修改个人信息：

	author :
	name : Name Lastname
	email : blah@email.test
	github : username
	twitter : username
	feedburner : feedname
	
3) 引用：_config.yml中的键值均引用到其他页面{{ site.title }}；

②、写文章

按照_config.yml的格式permalink: /:categories/:year/:month/:day/:title，可以修改格式，创建markdown格式文件，就可以当初博客发布了。

③、发布

本地预览修改：运行jekyll –server，预览http:127.0.0.1:4000。

发布到github上：通过命令提交或者客户端提交。

**4.6 个性化博客**

Github Page完成了博客的主要功能，写作、发布、修改，这些都是相对静态的东西，就是你自己可以控制的事情，还有一些动态的东西Github Pages无法支持，比如说文章浏览次数、文章的评论等，还有一些个性化的东西，像个性化页头页尾、代码高亮可以把博客整的更漂亮一点，其实这写都可以通过第三方应用来实现，个性化自己的博客。

加上Disqus云评论:

注册http://disqus.com
修改_config.yml:

	comments :
	provider : disqus
	disqus :
	short_name : tiansj

### 5 使用Markdown
**5.1 简介**
Markdown 的宗旨是实现「易读易写」。可读性，无论如何，都是最重要的。

Markdown 的语法全由一些符号所组成，这些符号经过精挑细选，其作用一目了然。格式撰写的文件可以直接以纯文本发布，并且看起来不会像是由许多标签或是格式指令所构成。

资料：[搭建环境](http://www.cnblogs.com/purediy/archive/2013/01/10/2855397.html)

**5.2 基本语法**

使用一个或多个空行分隔内容段来生成段落 <p>。
标题（h1~h6）格式为使用相应个数的“#”作前缀，比如以下代码表示 h3：
### this is a level-3 header ###

使用“>”作为段落前缀来标识引用文字段落。这其实是 email 中标记引用文字的标准方式：

> 引用的内容 *>* 这个记号直接借鉴的邮件标准

使用*“*”“+”“-”*来表示无序列表；使用数字加“.”表示有序列表。如：

1. I am ordered list item 1...
2. So I should be item 2


使用4个以上*空格*或1个以上的*tab*来标记代码段落，它们将被*<*pre*>*和*<*code*>*包裹，这意味着代码段内的字体会是 monospace家族的，并且特殊符号不会被转义。

```
使用 <[test](http://example.net "optional title")>来标记普通链接。
使用 <![img](http://example.net/img.png "optional title")>来标记图片。
引号内的 title 文字是可选的，链接也可以使用相对路径。
```

使用 * 或 _ 包裹文本产生 strong 效果：<p>
_语气很重的文本_ 以及 **语气更重的文本**

##...

### 参考资料
[GitHub Pages 建立个人主页](https://pages.github.com/)

[jekyll](http://jekyll.bootcss.com/)

[GitHub Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

[通过GitHub Pages建立个人站点(详细步骤)](http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html)

[基于jekyll-bootstrap的GitHub Pages搭建过程](http://www.cnblogs.com/Leo_wl/p/3402935.html)

[jekyll-bootstrap模板](http://layouts-the.me.s3-website-us-east-1.amazonaws.com/index.html)
