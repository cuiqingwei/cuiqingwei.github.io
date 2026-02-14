# iZheteng

基于hexo和yelee主题的个人blog

[![LICENSE](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square&label=LICENSE)](https://github.com/cuiqingwei/hexo-yelee/blob/master/LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/cuiqingwei/hexo-yelee.svg?style=flat-square&label=Stars)](https://github.com/cuiqingwei/hexo-yelee/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/cuiqingwei/hexo-yelee.svg?style=flat-square&label=Forks)](https://github.com/cuiqingwei/hexo-yelee/fork)

## Usage

- User Guide [Yelee — 简而不减 Hexo 双栏博客主题](http://moxfive.coding.me/yelee/)

1. 文章置顶

```
$ npm uninstall hexo-generator-index --save
$ npm install hexo-generator-index-pin-top --save
```

然后在需要置顶的文章的Front-matter中加上top: true即可。比如下面这篇文章：

```
---
title: hexo+GitHub博客搭建实战
date: 2017-09-08 12:00:25
categories: 博客搭建系列
top: true
---
```

2. 常见问题

git clone 代码 hexo -s 运行出错，请执行

```bash
rm package-lock.json
rm -rf node_modules
npm install

```

## Acknowledgments

- [hexojs/hexo](https://github.com/hexojs/hexo)
- [hexo-theme-yelee](https://github.com/MOxFIVE/hexo-theme-yelee)

## License

[MIT](https://github.com/cuiqingwei/hexo-yelee/blob/master/LICENSE) © GaryCUI
