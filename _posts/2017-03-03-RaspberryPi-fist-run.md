---
layout: post
title: 树莓派的初次启动设置
date: 2017-03-03
categories: blog
tags: [RaspberryPi]
description: 树莓派的初次启动设置
---

### 步骤1：打开设置界面

初次登录Raspbian系统时会自动打开设置的界面，需要根据实际情况进行一些简单的设置。如果是使用PuTTY原创登录树莓派或者已经不是第一次登录Raspbian系统，则不会自动打开设置界面，需要手动输入命令

`sudo raspi-config`

### 步骤2：扩展文件系统

如果你直接将系统镜像写入SD卡，SD卡的一部分空间不会被使用。在设置界面用上下方向键选择`Expand Filesystem`，然后用左右方向键选择 `<Select>`，点击回车键就会提示根分区扩展成功，需要重新启动。

### 步骤3：修改密码

选择 `Change User Password`，会显示一个简短的说明点击 `<OK>`，就会在命令行中提示输入密码，输入密码后不会在屏幕上有任何显示，会要求重复输入一次密码以确认。

### 步骤4：设置时区

选择 `Internationalisation Options`，接下来选择 `Change Timezone`。不同的时区会以地域进行划分，修改成为中国的时区，可以依次选择 `Asia` 和 `Chongqing`。

全部的设置完成后，选择 `<Finish>` 即可退出，修改部分设置会提示需要重新启动。

### 注：

使用 `raspi-config` 命令修改配置，其实是修改Linux的各种标准的配置文件，它提供了一个简单的图形界面方便修改最常用的配置。比如进行超频的话，会直接修改 `/boot/config.txt` 文件。