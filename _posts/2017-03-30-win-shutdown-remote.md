---
layout: post
title: 用命令实现Win7远程桌面关机和重启
date: 2017-03-30
categories: blog
tags: [windows]
description: 用命令实现Win7远程桌面关机和重启
---

打开运行框（Win+R键），输入下面命令，后面的数字表示关机/重启延迟的时间， 即可通过命令实现关机或重启：

	关机 shutdown -s -t 0 
	重启 shutdown -r -t 0

### shutdown 用法: 

	shutdown [-i | -l | -s | -r | -a] [-f] [-m \computername] [-t xx] [-c "comment"] [-d up:xx:yy]没有参数     显示此消息(与 ? 相同)

	-i               	显示 GUI 界面，必须是第一个选项
	-l               	注销(不能与选项 -m 一起使用)
	-s               	关闭此计算机
	-r               	关闭并重启动此计算机
	-a               	放弃系统关机
	-m \computername    远程计算机关机/重启动/放弃
	-f               	强制运行的应用程序关闭而没有警告
	-t 					时间：设置关机倒计时
	-c "消息内容"		输入关机对话框中的消息内容(不能超127个字符）

### 举例：

关闭计算机：

	shutdown –s

延迟3秒关闭计算机：

	shutdown –s –t 3

想要计算机在12点的时候关机：

	at 12:00 shutdown -s

**取消关闭计算机：**

	shutdown –a