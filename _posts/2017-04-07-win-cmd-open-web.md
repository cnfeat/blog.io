---
layout: post
title: win系统命令行下打开网页方法
date: 2017-04-07
categories: blog
tags: [windows]
description: win系统命令行下打开网页方法
---

本篇内容很简单，就是讲如何在windows系统下，用系统自带的命令行打开特定的网页的方法。一般会被使用在win服务器上，设定定时计划，定时运行一个cmd脚本，从而实现定时访问网页的目的。

### win7系统下：

	@echo off
	start  iexplore.exe http://www.test.com
	ping -n 15 127.0.0.1>nul
	taskkill /f /im iexplore.exe
	exit


### win2003系统下：

	@echo off
	start http://www.test.com
	ping -n 15 127.0.0.1>nul
	taskkill /f /im iexplore.exe
	exit

### 解析：

第二行的 `start` 的意思是调用系统默认的浏览器，打开后面紧跟的网址。

`ping -n 15 127.0.0.1>nul` 这行的作用是使脚本等待几秒钟时间，以防止因为网络延迟网页还没完全打开，就把浏览器进程给结束掉，从而访问网页失败。具体等待的时间，可以通过 -n 后面的数字来调节，想等待更久，就把数字调更大点。

需要注意的是，这个脚本里默认浏览器为IE，如果win服务器的默认浏览器不是IE，则为导致脚本运行失败。比如说你另外安装了Firefox浏览器，往往会将默认浏览器改成Firefox。