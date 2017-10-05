---
layout: default
title: "在树莓派2B上安装gentoo"
date: 2017-04-23 10:35:06
author: leetking
categories: gentoo
---

# 在树莓派2B上安装gentoo

## 一、对SD卡分区并格式化

第一个boot分区，大小64MB足够了，采用`fat32`分区格式。
余下就是linux分区，这里采用只有一个分区root，采用的`f2fs`文件格式，一种对flash存储介质有好的文件系统格式。
```bash
# fdisk -l /dev/sdb
Disk /dev/sdb: 3.7 GiB, 3930062848 bytes, 7675904 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000

/dev/sdb1  *      2048  133119  131072   64M  c W95 FAT32 (LBA)
/dev/sdb2       133120 7675903 7542784  3.6G 83 Linux
$ mount -l | grep sdb
/dev/sdb2 on /home/ultk/pi type f2fs (rw,relatime,lazytime,background_gc=on,user_xattr,acl,inline_data,inline_dentry,flush_merge,extent_cache,mode=adaptive,active_logs=6)
/dev/sdb1 on /home/ultk/pi/boot type vfat (rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro)
```

## 二、挂载分区到宿主系统
现在的mount可以不指定文件系统类型而可以自行确定。
```bash
# mount /dev/sdb2 ~/pi
# mkdir ~/pi/boot/
# mount /dev/sdb1 ~/pi/boot
```

## 三、安装gentoo和树莓派固件
这里由gentoo提供基本文件系统`stage3`包和包管理器`portage`，内核和引导固件由树莓派官方提供。
```bash
$ # 下载gentoo的stage3文件
$ wget http://mirrors.ustc.edu.cn/gentoo/releases/arm/autobuilds/current-stage3-armv7a_hardfp/stage3-armv7a_hardfp-20161129.tar.bz2 -O gentoo-stage3-armhf.tar.bz2
# tar -jxpf gentoo-stage3-armhf.tar.bz2 -C ~/pi/
$ # 下载并安装gentoo的portage系统
$ wget http://distfiles.gentoo.org/snapshots/portage-latest.tar.bz2
$ tar -jxpf portage-latest.tar.bz2 -C ~/pi/usr/
$ # 下载树莓派固件
$ git clone --depth 1 git://github.com/raspberrypi/firmware.git
$ cd firmware/boot
$ cp -r * ~/pi/boot/
$ cp -r ../modules ~/pi/lib/
```

## 四、配置系统
### 1. 编辑内核启动参数
`/etc/cmdline.txt`是内核启动参数配置文件。
```txt
```
### 2. 编辑`fstab`挂载文件系统

生成linux用户等里密码的加密值
$ openssl passwd -1

设置时区
 链接文件到localtime
设置语言
 在/etc/locale.gen里指明系统需要支持那些语言的处理，再使用locale.gen生成
然后采用不同的系统systemd或openrc有不同的方式设置系统的默认LANG

添加ntp

gentoo通过保存use、make.conf、还有/var/lib/portage/world，这样可以随时构建出原来的系统。

## TODO
1. f2fs性质
2. fstab编写挂在参数
3. cmdline.txt里一些参数不理解
4. ssh登录配置
5. emerge使用方法
6. 配置sshd
