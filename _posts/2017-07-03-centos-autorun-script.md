---
layout: post
title: CentOS开机自动运行自己的脚本
date: 2017-07-03
categories: blog
tags: [centos]
description: CentOS开机自动运行自己的脚本
---

Linux在启动时，会自动执行 `/etc/rc.d` 目录下的初始化程序，因此我们可以把启动任务放到该目录下，有下列办法：
 
### 方案一：

比较简单，就是上面的做法，`/etc/rc.d/`目录下的初始化程序很多，`rc.local`是在完成所有初始化之后执行的，所以在这里做手脚很合适。

root权限编辑`/etc/rc.d/rc.local`

	su
	cd /etc/rc.d/
	vi rc.local

在这个文件加上你要执行的脚本:

	#!/bin/sh  
	#  
	# This script will be executed *after* all the other init scripts.  
	# You can put your own initialization stuff in here if you don't  
	# want to do the full Sys V style init stuff.  

	# 下面内容是新添加的命令脚本
	touch /var/lock/subsys/local  
	mount //192.168.0.3/data2-1 /mnt/data2-1 -o username=un,password=123 

提示：这里的做法很不成熟，最好自己写个脚本文件在这里来调用，结构更清晰。

所以，更好的做法是：

我们在 `/etc/rc.d/rc.local` 文件里添加如下语句，调用需要执行的脚本：

	/home/run_script.sh

然后在 `run_script.sh` 文件里，再输入要执行的命令语句：

	touch /var/lock/subsys/local  
	mount //192.168.0.3/data2-1 /mnt/data2-1 -o username=un,password=123 
 
### 方案二：

`init.d` 目录下都为可执行程序，他们其实是服务脚本，按照一定格式编写，Linux 在启动时会自动执行，类似Windows下的服务。

 






