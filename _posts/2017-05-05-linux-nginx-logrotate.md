---
layout: post
title: 使用logrotate为nginx日志分卷
date: 2017-05-05
categories: blog
tags: [linux,nginx]
description: 使用logrotate为nginx日志分卷
---

> 日志文件包含了关于系统中发生的事件的有用信息，在排障过程中或者系统性能分析时经常被用到。对于忙碌的服务器，日志文件大小会增长极快，服务器会很快消耗磁盘空间，这成了个问题。除此之外，处理一个单个的庞大日志文件也常常是件十分棘手的事。


logrotate 是个十分有用的工具，它可以自动对日志进行截断（或轮循）、压缩以及删除旧的日志文件。

### 一、查看服务器上 Nginx pid 文件位置:

我的 Ubuntu 系统位置在 `/opt/nginx/logs/nginx.pid`

### 二、你需要知道这个关键进程：  

	# 这个命令是使 nginx 开始一个新日志
	kill -USR1 `cat /opt/nginx/logs/nginx.pid`

### 三、 新建立一个配置文件

	vi /etc/logrotate.d/nginx

文件内容如下： 

	# nginx SIGUSR1: Re-opens the log files.
	"/opt/nginx/logs/access.log" "/opt/nginx/logs/error.log" {
	daily
	rotate 7
	dateext
	copytruncate
	missingok
	notifempty
	delaycompress
	sharedscripts
	postrotate
		test ! -f /opt/nginx/logs/nginx.pid || kill -USR1 `cat /opt/nginx/logs/nginx.pid`
	endscript
	}

#### 命令解析：

	monthly: 	日志文件将按月轮循。其它可用值为‘daily’，‘weekly’或者‘yearly’。
	rotate 5: 	一次将存储5个归档日志。对于第六个归档，时间最久的归档将被删除。
	compress: 	在轮循任务完成后，已轮循的归档将使用gzip进行压缩。
	delaycompress: 	总是与compress选项一起用，delaycompress 选项指示 logrotate 不要将最近的归档压缩，压缩将在下一次轮循周期进行。这在你或任何软件仍然需要读取最新归档时很有用。
	missingok: 	在日志轮循期间，任何错误将被忽略，例如“文件无法找到”之类的错误。
	notifempty: 	如果日志文件为空，轮循不会进行。
	create 644 root root: 	以指定的权限创建全新的日志文件，同时 logrotate 也会重命名原始日志文件。
	postrotate/endscript: 	在所有其它指令完成后，postrotate 和 endscript 里面指定的命令将被执行。

上面的模板是通用的，而配置参数则根据你的需求进行调整，不是所有的参数都是必要的。

### 四、记住： logrotate 运行时，是有一个自己的状态记录文件的。

logrotate状态记录文件：`/var/lib/logrotate.status`

它会在这个文件中记录好每个文件的最早时间。然后每次运行时把目标文件跟自己的表做对比。如果发现目标文件的日期超过了这个表的日期，比如一个星期，那么 logrotate 才会对它分卷。 

### 五、 典型的命令：  

	logrotate -v /etc/logrotate.d/nginx

如果你使用了 `-vd` 那么就仅仅是一个 dry-run 预演，不会对 `/var/lib/logrotate.status` 生效。

`-d` 选项：	排障过程中的最佳选择，以预演方式运行 logrotate。不会实际轮循任何日志文件，可以模拟演练日志轮循并显示其输出。

`-f` 选项：	即使轮循条件没有满足，强制logrotate轮循日志文件。

`-v` 选项：	提供详细的输出。

### 六、 把 logrotate 增加到你的 crontab 定时计划中去。

	crontab -e

例如增加以下内容（每天的0点执行日志分卷）：

	0 0 * * * logrotate -v /etc/logrotate.conf

如果你想要改动立即生效的话：

#### 1、先运行 logrotate : 

	logrotate -v /etc/logrotate.d/nginx

#### 2、编辑 `/var/lib/logrotate.status`

把里面对应的内容，改成昨天。

	vim /var/lib/logrotate.status

例如：`"/usr/local/nginx/logs/access.log" 2013-12-22`

要改成： `"/usr/local/nginx/logs/access.log" 2013-12-21`

#### 3、再运行：

	logrotate -v /etc/logrotate.d/nginx