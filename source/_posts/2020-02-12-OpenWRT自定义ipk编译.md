---
layout: post
title: 'OpenWRT自定义ipk编译'
date: 2020-02-12 21:37
comments: true
tags:
  - OpenWRT
  - 备忘
---

### Preparing, configuring and building the necessary tools

```shell
cd ~
mkdir buidbot
cd ~/buildbot
git clone https://git.openwrt.org/openwrt/openwrt.git source
cd /home/buildbot/source
git checkout v17.01.2
make distclean
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
make toolchain/install
```

```shell
export PATH=/home/buildbot/source/staging_dir/host/bin:$PATH
```

### Creating a package feed for your packages

```bash
mkdir -p mypackages/examples/helloworld
```

### Creating the package manifest file

```shell
cd home/buildbot/mypackages/examples/helloworld
touch Makefile
```

<!--more-->

```makefile
include $(TOPDIR)/rules.mk

# Name, version and release number
# The name and version of your package are used to define the variable to point to the build directory of your package: $(PKG_BUILD_DIR)
PKG_NAME:=helloworld
PKG_VERSION:=1.0
PKG_RELEASE:=1

# Source settings (i.e. where to find the source codes)
# This is a custom variable, used below
SOURCE_DIR:=/home/buildbot/helloworld

include $(INCLUDE_DIR)/package.mk

# Package definition; instructs on how and where our package will appear in the overall configuration menu ('make menuconfig')
define Package/helloworld
  SECTION:=examples
  CATEGORY:=Examples
  TITLE:=Hello, World!
endef

# Package description; a more verbose description on what our package does
define Package/helloworld/description
  A simple "Hello, world!" -application.
endef

# Package preparation instructions; create the build directory and copy the source code.
# The last command is necessary to ensure our preparation instructions remain compatible with the patching system.
define Build/Prepare
        mkdir -p $(PKG_BUILD_DIR)
        cp $(SOURCE_DIR)/* $(PKG_BUILD_DIR)
        $(Build/Patch)
endef

# Package build instructions; invoke the target-specific compiler to first compile the source file, and then to link the file into the final executable
define Build/Compile
        $(TARGET_CC) $(TARGET_CFLAGS) -o $(PKG_BUILD_DIR)/helloworld.o -c $(PKG_BUILD_DIR)/helloworld.c
        $(TARGET_CC) $(TARGET_LDFLAGS) -o $(PKG_BUILD_DIR)/$1 $(PKG_BUILD_DIR)/helloworld.o
endef

# Package install instructions; create a directory inside the package to hold our executable, and then copy the executable we built previously into the folder
define Package/helloworld/install
        $(INSTALL_DIR) $(1)/usr/bin
        $(INSTALL_BIN) $(PKG_BUILD_DIR)/helloworld $(1)/usr/bin
endef

# This command is always the last, it uses the definitions and variables we give above in order to get the job done
$(eval $(call BuildPackage,helloworld))
```

### Including the new package feed into the OpenWrt build system

```shell
cd /home/buildbot/source
touch feeds.conf
```

```shell
src-link mypackages /home/buildbot/mypackages
```

### Updating and installing feeds

```shell
cd /home/buildbot/source
./scripts/feeds update mypackages
./scripts/feeds install -a -p mypackages
```

### Building the package

```shell
make package/helloworld/compile
```

If everything went successfully, we are presented with a brand new package named `helloworld_1.0-1_.ipk` in `bin/packages//mypackages` folder.

### Deploying and testing your package

```bash
root@ubuntu:/# scp helloworld_1.0-1_<arch>.ipk root@192.168.0.1:/tmp/
```

```bash
root@OpenWrt:/# opkg install /tmp/helloworld_1.0-1_<arch>.ipk
Installing helloworld (1.0-1) to root...
Configuring helloworld.
```

```shell
root@OpenWrt:/# helloworld
Hello, world!
```

### Removing your package

```shell
root@OpenWrt:/# opkg remove helloworld
Removing package helloworld from root...
```

```shell
root@OpenWrt:/# rm /tmp/helloworld_1.0-1_<arch>.ipk
```

["Hello, world!" for OpenWrt](https://openwrt.org/docs/guide-developer/helloworld/start)
