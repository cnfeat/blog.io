---
layout: post
title: 修改树莓派操作系统Raspbian的软件源
date: 2017-02-24
categories: blog
tags: [RaspberryPi]
description: 修改树莓派操作系统Raspbian的软件源
---

> 注：Raspbian 是为树莓派 (Raspberry Pi) 设计，基于Debian的操作系统，由一个小团队开发。其不隶属于树莓派基金会，但被列为官方支持的操作系统。

### 步骤1：登录到Raspbian的命令行界面

### 步骤2：修改Raspbian的软件源

> 软件源是Linux系统免费的应用程序安装仓库，很多的应用软件都会这收录到这个仓库里面。直接使用软件源中的软件进行安装就无需自行编译，这对于速度不快的树莓派来说能节省不少时间。

软件源配置文件为 `/etc/apt/sources.list`，里面写了你所用的软件源地址（注意不是软件安装包在这个文件夹，而是仅有服务器的描述信息，所有的软件安装获取还是需要联网的）。 

Rasbpian维护一份官方的软件源，这就是默认写在配置文件中的地址，然而官方的软件源在国外，下载速度很慢。一些大学和企业由于需要大量下载软件，或者为了公益目的，就会下载到自己的服务器上，提供更快的下载速度。

在中国Raspbian的软件源有： 

中国科技大学： [http://mirrors.ustc.edu.cn/raspbian/raspbian/](http://mirrors.ustc.edu.cn/raspbian/raspbian/) 

清华大学： [http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ ](http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/)

浙江大学： [http://mirrors.zju.edu.cn/raspbian/raspbian/](http://mirrors.zju.edu.cn/raspbian/raspbian/) 

[官方认证的Raspbian软件源列表](http://www.raspbian.org/RaspbianMirrors) 

如果你需要修改软件源，直接编辑配置文件即可 

`sudo vi /etc/apt/sources.list`

比如使用中国科技大学的软件源，就可以修改成：

	deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ wheezy main contrib non-free  
	#deb-srchttp://mirrors.ustc.edu.cn/raspbian/raspbian/ wheezy main contrib non-free

### 步骤3：修改树莓派的软件源

除了Raspbian的软件源外，树莓派还有一份专门的软件源，配置文件位于 `/etc/apt/sources.list.d/raspi.list` 。这个软件源的镜像要少一些。

直接编辑配置文件

`sudo vi /etc/apt/sources.list.d/raspi.list`

比如使用中国科技大学的软件源，就可以修改成：

	deb http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ wheezy main  
	#deb-src http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ wheezy main
