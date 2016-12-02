---
layout: post
title: Linux 系统下挂载 NTFS 格式 移动硬盘方法
date: 2016-12-02
categories: blog
tags: [linux]
description: Linux 系统下挂载 NTFS 格式 移动硬盘方法
---

### 一、使用 `modprobe fuse` 命令查看系统是否支持FUSE

若没有显示 `FATAL: Module fuse not found`

表示您可以使用 ntfs3g 进行移动硬盘挂载了。

### 二、确认 fuse 是否已安装

CentOS 5.5 带有fuse，可以使用 `rpm -qa | grep fuse` 查看是否安装。

    [root@localhost]# rpm -qa | grep fuse
    fuse-2.7.4-8.el5

说明已经安装fuse

如果没有显示，请执行以下命令安装：

`[root@localhost]# yum install fuse`
 
### 三、安装 ntfs-3g 支持模块

如果系统默认的软件库更新不到 ntfs-3g，可以自己下载编译安装。

软件项目：[http://www.tuxera.com/community/open-source-ntfs-3g/](http://www.tuxera.com/community/open-source-ntfs-3g/)

下载：[https://tuxera.com/opensource/ntfs-3g_ntfsprogs-2016.2.22.tgz](https://tuxera.com/opensource/ntfs-3g_ntfsprogs-2016.2.22.tgz)

命令行下载：

`wget https://tuxera.com/opensource/ntfs-3g_ntfsprogs-2016.2.22.tgz`

编译安装：

    tar -zxvf nntfs-3g_ntfsprogs-2016.2.22.tgz
    cd ntfs-3g_ntfsprogs-2016.2.22.tgz

    ./configure
    make
    make install

### 四、挂载移动硬盘

`mount -t ntfs-3g /dev/sdb1 /mnt/usb1`

语法：

`mount -t ntfs-3g <NTFS Partition> <Mount Point>`

### 五、卸载移动硬盘

`umount /mnt/usb1`


### 备注：

对 **ntfs 格式** 的磁盘分区应使用 **-t ntfs-3g** 参数

对 **fat32 格式** 的磁盘分区应使用 **-t vfat** 参数。

若汉字文件名显示为乱码或不显示，可以使用下面的命令格式。（增加 **-o iocharset=cp936** 参数）

`# mount -t ntfs-3g -o iocharset=cp936 /dev/sdb1 /mnt/usb1`