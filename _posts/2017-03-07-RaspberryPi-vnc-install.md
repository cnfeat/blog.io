---
layout: post
title: 树莓派(raspberry pi)安装VNC
date: 2017-03-07
categories: blog
tags: [RaspberryPi]
description: 树莓派(raspberry pi)安装VNC
---

> VNC 是一款优秀的远程控制软件，开源的。在树莓派上安装VNC，就可以远程控制了。

### 一、首先，在树莓派上安装Tight VNC 包

`sudo apt-get install tightvncserver`

### 二、启动VNC服务端，命令如下

	vncserver
	vncserver :1

当提示输入密码时，创建一个密码 (这个密码是远程用户访问时用的）

### 三、在远程计算机上访问

#### 第一种：win系统客户端安装：

下载 VNC Viewer，地址：[http://www.tightvnc.com/download.php](http://www.tightvnc.com/download.php)

下载Window版的 VNC-Viewer ，无需安装，解压即可。 比如，解压出：VNC-Viewer-5.0.3-Windows-32bit.exe

运行 `VNC-Viewer`


在 VNC Server栏中，输入树莓派的IP地址，**后面加上 :1 字样**，如 `192.168.1.103:1`

按 connect 连接

当提示输入密码时，输入之前定义的密码

连接OK后，出现树莓派的窗口系统了，你就可以远程控制它了。

#### 第二种：Ubuntu系统客户端安装：

通过命令行安装：

	sudo apt-get install xtightvncviewer

运行客户端：

	xtightvncviewer

输入树莓派的IP地址，按 connect 连接：

	192.168.1.103:1
