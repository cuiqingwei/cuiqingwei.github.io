---
layout: post
title: '利用Travis CI、MkDocs自動部署Blog至GitHub Pages'
date: 2016-10-27 11:01:40
comments: true
tags:
  - Travis CI
  - MkDocs
  - GitHub
  - GitHub Pages
---

# 技术要点

## GitHub Pages

[GitHub Pages](https://pages.github.com/)免费的静态站点，三个特点：免费托管、自带主题、支持自制页面和Jekyll。

## MkDocs

[MkDocs](http://markdown-docs-zh.readthedocs.io/zh_CN/latest/)是一个用于创建项目文档的 快速, 简单 , 完美华丽 的静态站点生成器. 文档源码使用 Markdown 来撰写, 用一个 YAML 文件作为配置文档.

## Travis CI

[Travis CI](https://travis-ci.org/)，是一个专门为开源项目打造的持续集成环境。
如果你有一个放在github上的开源项目，Travis CI简直就是一个完美的CI选择。

<!--more-->

# 实现过程

## 说明

源码存放处 [https://github.com/cuiqingwei/iotknowledges](https://github.com/cuiqingwei/iotknowledges)
页面存放处 [https://github.com/iotknowledges/iotknowledges.github.io](https://github.com/iotknowledges/iotknowledges.github.io)

<font color="red">通过 **Travis CI** 部署的 **MkDocs** 环境将 **iotknowledges** 源码编译成静态页面并发布到 **iotknowledges.github.io**</font>

## 1.创建一个GitHub Pages

1.1 安装git工具
http://windows.github.com/
http://mac.github.com/

1.2 两种pages模式

    ① User/Organization Pages 个人或公司站点
      - 1) 使用自己的用户名，每个用户名下面只能建立一个；
      - 2) 资源命名必须符合这样的规则username/username.github.com；
      - 3) 主干上内容被用来构建和发布页面.
    ② Project Pages 项目站点
      - 1) gh-pages 分支用于构建和发布；
      - 2) 如果user/org pages使用了独立域名，那么托管在账户下的所有project pages将
           使用相同的域名进行重定向，除非project pages使用了自己的独立域名；
      - 3) 如果没有使用独立域名，project pages将通过子路径的形式提供服务username.github.com/projectname；
      - 4) 自定义404页面只能在独立域名下使用，否则会使用User Pages 404；
      - 5) 创建项目站点步骤：
    ```
    $ git clone https://github.com/USERNAME/PROJECT.git PROJECT
    $ git checkout --orphan gh-pages
    $ git rm -rf .
    $ git add .
    $ git commit -a -m "First pages commit"
    $ git push origin gh-pages
    ```

1.3. 可以通过User/Organization Pages建立主站，而通过Project Pages挂载二级应用页面。

<font color="blue">由于我是按照Organization名字命名的Pages，Repository名字是 _iotknowledges.github.io_</font>

![](/img/20161027/github1.png)
![](/img/20161027/github2.png)

<font color="blue">**至此，Project Pages建立完成，用来存放MkDocs生成的静态页面！**</font>

## 2. 建立存放源码的Repository并关联Travis CI

2.1 创建iotknowledges

登录GitHub，点击 **New repository** 新建即可，具体方法步骤多说！

2.2 生成Token

在GitHub账户的Settings里生成

![](/img/20161027/github3.png)
![](/img/20161027/github4.png)

## 3. Travis CI工程的Settings,添加环境变量

登录 [https://travis-ci.org/](https://travis-ci.org/) 点击 <font color="green"> **Sign in with GitHub** </font> 用存放源码的github账号登录

添加一个环境变量(Environment Variable)，命名为 **GITHUB_API_KEY** ，之后将刚刚生成的Token复制进去，这在之后会用到。接下来你还需要创建两个环境变量作为Push的身份，一个是你的名字，还有一个是你的邮箱，当然如果对邮箱和名字没什么隐私那不设置也可以。这两个变量可以分别命名为 **GITHUB_EMAIL** 和 **GITHUB_NAME**。GITHUB_NAME请务必注意设置成你Profile链接里的那个名字，即https://github.com/[user_name]这里这个值。设置完之后，注意要把下面那个“*Display value in build log*”关掉，否则这一安全措施就没用了。还有一点，记得把“*Build pull requests\*”关掉，否则在有Pull Request的时候不论其是否合格都会自动将Pages更新。

![](/img/20161027/github5.png)

## 4. 配置.travis.yml

由于MkDocs使用的是Python，所以先在文件开头加上：

```
language: python
python:
-  3.5
```

这里我使用的是Python 3.5，如果有插件不兼容的话可以切换成2.7。之后是初始化设置：

```
install:
  - pip install mkdocs
```

接下来是构建：

```
script:
  - mkdocs build --clean
```

生成的最终网站会在site目录内，--clean参数保证构建的时候site文件夹内没有多余文件，否则可能会导致各种奇怪的问题。

接下来就是最重要的部署环节了，脚本需要放在after_success阶段内：

```
after_success:  |
  if [ -n "$GITHUB_API_KEY" ]; then
    cd "$TRAVIS_BUILD_DIR"
    cd site
    git init
    git add .
    git -c user.name=$GITHUB_NAME -c user.email=$GITHUB_EMAIL commit -m "Auto Deployment"
    git push -f -q https://$GITHUB_NAME:$GITHUB_API_KEY@github.com/[user_name]/[repo_name] [branch]
    cd "$TRAVIS_BUILD_DIR"
  fi
```

首先上来先要保证 **GITHUB_API_KEY** 这个环境变量存在，之后进入到构建目录，再用相对位置进入构建好的site网站目录。接着就直接在本地创建一个repo，然后commit，push到需要的repo。注意[user_name]是repo所在的用户名，[repo_name]是GitHub Pages所在的repo。[branch]这里如果是用户或者Organization的GitHub Pages的话就填 master，如果是工程的GitHub Pages的话就填gh-pages。最后返回构建目录。

由于push的时候有<font color="red">**-f**</font>参数，GitHub上的历史都将被复写，并且因为我们是本地新创建的一个repo，所有历史都是空的，所以构建完成之后每次在GitHub上都只能看到一个commit ，如果成功的话，应该每次更新源码工程之后等一两分钟都能看到你自己在GitHub Pages所在的工程或者分支自动发送的commit，如果失败的话可以看一下下面完整的 **.travis.yml** 文件：

```
language: python
python:
  -  3.5
install:
  - pip install mkdocs
script:
  - mkdocs build --clean
after_success:  |
  if [ -n "$GITHUB_API_KEY" ]; then
    cd "$TRAVIS_BUILD_DIR"
    cd site
    git init
    git add .
    git -c user.name=$GITHUB_NAME -c user.email=$GITHUB_EMAIL commit -m "Auto Deployment"
    git push -f -q https://$GITHUB_NAME:$GITHUB_API_KEY@github.com/iotknowledges/iotknowledges.github.io master
    cd "$TRAVIS_BUILD_DIR"
  fi
```

5. build 状态，提交代码后可以到 https://travis-ci.org/ 查看进度情况

![](/img/20161027/github6.png)

[https://iotknowledges.github.io/](https://iotknowledges.github.io/)
