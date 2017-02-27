---
layout: post
title: 通过远程桌面连接树莓派
date: 2017-02-27
categories: blog
tags: [RaspberryPi]
description: 通过远程桌面连接树莓派
---

### 步骤1：使用PuTTY连接树莓派

### 步骤2：安装xrdp

> xrdp是一个支持远程桌面协议进行桌面共享的软件，能够完美地支持windows系统。 

在PuTTY中输入以下命令安装xrdp：

`sudo apt-get install xrdp`

### 步骤3：进行远程桌面连接

打开 开始菜单 > 点击运行 > 输入 `mstsc` 打开远程桌面连接的客户端。

> 注：mstsc是MiscroSoft Terminal Service Client的缩写。

然后需要输入树莓派的IP地址，并点击连接。 

第一次连接会提示“无法验证此计算机的身份”，点击是确认即可。 

接下来需要输入用户名和密码，这和PuTTY登录树莓派的用户名密码是相同的，默认用户名是 `pi`，密码是 `raspberry`。

点击 OK 就大功告成了。 

xrdp会自动开机启动，安装好xrdp后，下一次进行远程桌面连接就只需进行步骤3。

注：

在Linux系统下，也可以使用这个协议进行连接。

运行 Remmina 远程桌面客户端， 选择 RDP，输入 IP 即可。