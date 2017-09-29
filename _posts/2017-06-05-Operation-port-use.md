---
layout: post
title: 如何查看端口是被哪个应用/进程占用
date: 2017-06-05
categories: blog
tags: [operations]
description: 如何查看端口是被哪个应用/进程占用
---

> 有时启动应用时会发现端口已经被占用，或者是感觉有些端口自己没有使用却发现是打开的。这时我们希望知道是哪个应用/进程在使用该端口。
 
CentOS 等 Linux 系统下可以用 `netstat` 或者 `lsof` 查看，Windows 下也可以用 `netstat` 查看，不过参数不同。

## Linux:
 
	netstat -nap 

列出所有正在使用的端口及关联的进程/应用

	lsof -i :portnumber 

portnumber 要用具体的端口号代替，可以直接列出该端口所使用的进程/应用

 
### 一、检查端口被哪个进程占用
 
	netstat -lnp|grep 88   #88请换为你的 apache 需要的端口，如：80
 
执行以上命令，可以查看到88端口正在被哪个进程使用。

 
### 二、查看进程的详细信息
 
	ps 1777  # 1777替换为需要查看的进程号
 
执行以上命令。查看相应进程号的程序详细路径。

 
### 三、杀掉进程，重新启动apache
 
	kill -9 1777        	#杀掉编号为1777的进程（请根据实际情况输入）
	service httpd start 	#启动apache
 
执行以上命令，如果没有问题，apache将可以正常启动。

 
## Windows系统:
 
	netstat -nao 	#会列出端口关联的的进程号，可以通过任务管理器查看是哪个任务
 
最后一列为程序PID，再通过 `tasklist` 命令：

	tasklist | findstr 2724
 
再通过任务管理结束掉这个程序就可以了