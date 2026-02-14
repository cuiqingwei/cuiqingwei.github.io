---
layout: post
title: '使用GitHub Actions编译OpenWRT'
date: 2019-12-24 12:07
comments: true
tags:
  - ubuntu
  - GitHub
---

![《使用 GitHub Actions 编译 OpenWrt》](/img/20191224/Actions.jpeg)

**Github Ac­tions** 是 GitHub 推出的持续集成 (Con­tin­u­ous in­te­gra­tion，简称 **CI**) 服务，它提供了配置非常不错的虚拟服务器环境（E5 2vCPU/7G RAM），基于它可以进行构建、测试、打包、部署项目。这篇文章就是介绍如何使用P3TERX 项目（work­flow文件），进行云编译 OpenWrt固件。该方法免费，快速，无需消耗自己的计算机资源，每次编译都是全新环境。

<!--more-->

项目地址：

https://github.com/P3TERX/Actions-OpenWrt （P3TERX 项目）

[https://github.com/cuiqingwei/Actions-OpenWrt](https://github.com/cuiqingwei/Actions-OpenWrt) （Fork项目）

## 一、拷贝项目

登录自己的 GitHub，**Fork**以上任一项目到自己GitHub，有其它需求可自行修改 work­flow 文件，方法后面有说明。

## 二、修改 work­flow 文件，配置触发编译

默认有两种方法触发编译：

- 通过在 `releases` 页面发布一个 release ；

- 把定生成好的`.config`文件push 到项目根目录下，这两种方法触发编译。

另个还可以修改work­flow文件使其增加定时`自动编译`、点击` Star` 开始编译以及通过SSH 连接到 Actions生成.config编译配置，或者其他定制化操作。

1、定时自动编译

编辑 work­flow 文件（.github/workflows/build-openwrt.yml）取消注释下面两行。

```
#  schedule:
#    - cron: 0 8 * * 5
```

例子是北京时间每周五下午 4 点钟开始编译。如需自定义则按照 cron 格式修改即可，Ac­tions 虚拟环境中的时区是 UTC ，注意按照自己所在地时区进行转换。

2、点击 Star 开始编译

编辑 work­flow 文件（.github/workflows/build-openwrt.yml）取消注释下面两行，后续点击自己仓库上的 `star `即可开始编译。

```
#  watch:
#    types: [started]
```

注意：编译完成后，不要忘记注释回去。以防，其他人点击`star`触发编译造成资源浪费。

## 三、定制化编译

1、DIY 脚本

仓库内有一个 `diy.sh` 文件，你可以把对源码修改的指令写到这个文件中，比如修改默认 IP、主机名、主题、添加 / 删除软件包等操作。但不仅限于这些操作，发挥你强大的想象力，可做出更强大的功能。

如：修改默认 IP为`192.168.10.1`

```
sed -i 's/192.168.1.1/192.168.10.1/g' package/base-files/files/bin/config_generate
```

删除原主题

```
rm -rf package/lean/luci-theme-argon
```

添加新的主题

```
git clone https://github.com/sypopo/luci-theme-atmaterial.git package/lean/luci-theme-atmaterial
```

取消bootstrap为默认主题

```
sed -i '/set luci.main.mediaurlbase=\/luci-static\/bootstrap/d' feeds/luci/themes/luci-theme-bootstrap/root/etc/uci-defaults/30_luci-theme-bootstrap
```

删除默认密码

```
sed -i "/CYXluq4wUazHjmCDBCqXF/d" package/lean/default-settings/files/zzz-default-settings
```

生成.config文件时记得选中相应的软件或主题。如果添加的软件包与 Open­Wrt 中已有的软件包同名的情况，则需要把源码中的同名软件包删除，否则会优先编译 Open­Wrt 中的软件包。

2、SSH 连接到 Actions生成编译配置

通过 SSH 连接到 GitHub Ac­tions 虚拟服务器环境，可直接进行 make menuconfig 操作生成编译配置，或者任意的定制化操作。

编辑 workflow 文件（.github/workflows/build-openwrt.yml），取消注释下面两行开启这个功能。

```
#    - name: SSH connection to Actions
#      uses: p3terx/debugger-action@master
```

在触发工作流程后，在 Actions 页面等待执行到SSH connection to Actions步骤，会出现类似下面的信息。

```
To connect to this session copy-n-paste the following into a terminal or browser:

ssh gnhyDLyDDueFaTDe223KyBbun@nyc1.tmate.io

https://tmate.io/t/Y26QeagDtsPXp2mT6me5cnMRd
```

复制 SSH 连接命令粘贴到终端内执行，然后输入以下命令进行编译配置。

```
cd openwrt && make menuconfig
```

完成后按快捷键`Ctrl+D`或输入`exit`命令退出，后续编译工作将自动进行。

3、Custom files（自定义文件）

俗称 “files 大法”实际就是把现在的配置以文件的形式固化到新固件里。这样做的好处就是升级不需要保留配置，或者恢复出厂设置，缺省值就是你自己设置好的配置。

如固化你的network设置： 首先提取路由固件下的\etc\config\network 然后在项目根目录下创建files目录并push 到到 \files\etc\config\network ，最后编译出来的固件就是你自己配置好的network。

定制化编译的方法可以搭配使用，自己需要的功能一般也是固定的，就可以在 make menuconfig 选好后执行 ./scripts/diffconfig.sh > seed.config 复制一下这个seed.config的文本内容到项目根目录的.config文件中，这样就不用每次都SSH 连接到 Actions生成编译配置，真正一键编译。另外如果，使用“files 大法”仓库最好设为私有，否则你的配置信息，如宽带账号等会公开在网上。

## 四、下载编译好的固件·

编译时间*1-2*小时，编译完成点击 `Actions` 页面右上角的`Artifacts`按钮下载固件。

![githubaction](/img/20191224/githubaction.png)
