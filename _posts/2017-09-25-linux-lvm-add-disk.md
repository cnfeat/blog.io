---
layout: post
title: CentOS7 LVM 添加硬盘及扩容
date: 2017-09-25
categories: blog
tags: [linux,centos,lvm]
description: CentOS7 LVM 添加硬盘及扩容
---

### 一、LVM简介

`LVM` 是 `Logical Volume Manager`（逻辑卷管理）的简写，它是 Linux 环境下对磁盘分区进行管理的一种机制。**LVM 将一个或多个磁盘分区（PV）虚拟为一个卷组（VG），相当于一个大的硬盘**，我们可以在上面划分一些逻辑卷（LV）。当卷组的空间不够使用时，可以将新的磁盘分区加入进来。我们还可以从卷组剩余空间上划分一些空间给空间不够用的逻辑卷使用。从而实现**在不丢失资料的情况下，动态增加磁盘分区的空间！**

## 二、LVM添加硬盘和扩容

测试环境：CentOS7 64位（KVM虚拟机）

LVM版本：lvm2-2.02.105-14.el7.x86_64

### 1、添加一块硬盘（8GB）到系统中

使用 `fdisk -l` 查看到这块新盘为 `/dev/vdb`：

	shell# fdisk -l

### 2、对新盘分区

使用 `fdisk` 命令对新盘进行分区，这里建立了一个主分区 `/dev/vdb1`，大小8GB，最后使用 `partprobe` 命令重新读取分区表：

	shell# fdisk /dev/vdb
	shell# partprobe

![](https://azraelgreen.github.io/img/20170925_01.jpg)

**在分区的过程中，注意设置格式为8e，这是LVM的分区格式。**

### 3、创建物理卷（PV）

使用 `pvcreate` 命令创建物理卷，`pvdisplay` 查看物理卷信息：

	shell# pvcreate /dev/vdb1
	shell# pvdisplay

### 4、将PV加入卷组（VG）

使用 `vgdisplay` 查看卷组信息，示例使用的卷组名为 centos，空闲大小为0：

	shell# vgdisplay

使用 `vgextend` 命令把 `/dev/vdb1` 加入到 centos：

	shell# vgextend centos /dev/vdb1

我们重新查看一下卷组信息，发现空闲空间为 8GB，说明 `/dev/vdb1` 已经成功加入进来：

### 5、创建逻辑卷（LV）

**PS：如果是直接添加到已有的逻辑卷，直接跳到第7步！**

使用 `lvcreate` 命令从卷组里划分一个新的逻辑卷，这里创建了名称为 `newlv`，大小 4GB 的逻辑卷分区；使用 `lvdisplay` 查看逻辑卷信息：

	shell# lvcreate -L 4G -n newlv centos
	shell# lvdisplay

我们再查看一下卷组信息，卷组剩余4GB空间了

### 6、格式化逻辑卷并挂载

新逻辑卷经过格式化就可以挂载到系统里存储数据了。使用 `mkfs.xfs` 格式化为 CentOS7 的xfs文件系统：

	shell# mkfs.xfs /dev/centos/newlv

挂载到/mnt目录下（你可以挂载到自己需要的目录下），挂载后看到容量为4GB：

	shell# mount -t xfs /dev/centos/newlv /mnt/
	shell# df -Th

设置开机自动挂载，编辑 /etc/fstab 文件，加入最后一行：

	/dev/centos/newlv       /mnt                    xfs     defaults        1 2

### 7、逻辑卷扩容

使用 `lvextend` 命令进行逻辑卷扩容。我把所有剩余空间都分配给了 newlv，增大到了8GB：

	shell# lvextend -l +100%FREE /dev/centos/newlv

使用 `xfs_growfs` 命令在线调整 xfs 格式文件系统大小（**CentOS6使用 `resize2fs`**）：

	(CentOS7)shell# xfs_growfs /dev/centos/newlv
	(CentOS6) shell# resize2fs /dev/centos/newlv

最后我们看到逻辑卷分区已经动态扩容到了8GB。
