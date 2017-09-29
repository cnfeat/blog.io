---
layout: post
title: VirtualBox安装Ubuntu解决分辨率太小问题
date: 2017-03-27
categories: blog
tags: [virtualbox]
description: VirtualBox安装Ubuntu解决分辨率太小问题
---

### 问题：

默认在VirtualBox软件里安装好Ubuntu14.04版本的虚拟主机后，重启进入Ubuntu系统，发现分辨率好小，只有640x480!在系统的分辨率选项里也无法修改大小。

### 解决办法：

这个问题光通过Ubuntu本身系统设置是无法解决的！需要额外安装VirtualBox系统增强工具。

选择VirutalBox的菜单 设备 -- 安装增强功能 ， 按照提示开始安装。

安装完毕后，从右上角系统菜单选择关机，退出，然后重启虚拟机。

再次进入Ubuntu系统，此时就可以任意修改分辨率了。