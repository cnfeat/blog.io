---
layout: post
title: 使用ab对nginx进行压力测试
date: 2017-07-19
categories: blog
tags: [nginx]
description: 使用ab对nginx进行压力测试
---

nginx以高并发，省内存著称。

ab是针对apache的性能测试工具，但也可以对 nginx 进行测试。可以只安装ab工具。

### 一、安装ab方法
ubuntu安装ab

	apt-get install apache2-utils

centos安装ab

	yum install httpd-tools

### 二、测试准备

测试之前可以准备一个简单的html、一个php、一个图片文件。分别对他们进行测试。

我们把这个三个文件放到 nginx 安装目录默认的html目录下，准备之后我们就可以测试了：

	ab -kc 1000 -n 1000 http://localhost/ab.html

这个指令会使用1000个并发，进行连接1000次。结果如下：

	root@~# ab -kc 1000 -n 1000 http://www.nginx.cn/ab.html

	This is ApacheBench, Version 2.3 &lt;$Revision: 655654 $&gt;
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/
 
	Benchmarking www.nginx.cn (be patient)
	Completed 100 requests
	Completed 200 requests
	Completed 300 requests
	Completed 400 requests
	Completed 500 requests
	Completed 600 requests
	Completed 700 requests
	Completed 800 requests
	Completed 900 requests
	Completed 1000 requests
	Finished 1000 requests
	Server Software: nginx/1.2.3
	Server Hostname: www.nginx.cn
	Server Port: 80
 
	Document Path: /ab.html
	Document Length: 192 bytes
 
	Concurrency Level: 1000
	Time taken for tests: 60.444 seconds
	Complete requests: 1000
	Failed requests: 139
	(Connect: 0, Receive: 0, Length: 139, Exceptions: 0)
	Write errors: 0
	Non-2xx responses: 1000
	Keep-Alive requests: 0
	Total transferred: 732192 bytes
	HTML transferred: 539083 bytes

	...

对于php文件和图片文件可以使用同样指令进行。
 
	ab -kc 500 -n 5000 http://localhost/ab.php
	ab -kc 500 -n 5000 http://localhost/ab.gif

### 三、结果说明
 
输出结果我们可以从字面意思就可以理解。这里对两个比较重要的指标做下说明

比如

	Requests per second: 16.54 [#/sec] (mean)

表示当前测试的**服务器每秒可以处理16.54个静态html的请求事务**，后面的mean表示平均。

这个数值表示当前机器的整体性能，值越大越好。

	Time per request: 60443.585 [ms] (mean)

单个并发的延迟时间，后面的mean表示平均。

隔离开当前并发，单独完成一个请求需要的平均时间。

#### 两个Time per request区别

	Time per request: 60443.585 [ms] (mean)
	Time per request: 60.444 [ms] (mean, across all concurrent requests)

**前一个衡量单个请求的延迟。**cpu是分时间片轮流执行请求的，多并发的情况下，一个并发上的请求时需要等待这么长时间才能得到下一个时间片。

通俗点说就是当以 `-c 10` 的并发下完成 `-n 1000` 个请求的同时，额外加入一个请求，完成这个请求平均需要的时间。

**后一个衡量性能的标准，**它反映了完成一个请求需要的平均时间。在当前的并发情况下，增加一个请求需要的时间。

通俗点说就是当以 `-c 10` 的并发下完成 `-n 1001` 个请求时，比完成 `-n1000` 个请求多花的时间。

你可以适当调节-c 和-n大小来测试服务器性能，借助 `htop` 指令来直观的查看机器的负载情况。

### 四、ab的参数详细解释

普通的测试，使用 `-c -n` 参数配合就可以完成任务

	格式： ./ab [options] [http://]hostname[:port]/path

参数：

	-n 测试的总请求数。默认时，仅执行一个请求
	-c 一次并发请求个数。默认是一次一个。

	-H 添加请求头，例如 ‘Accept-Encoding: gzip’，以gzip方式请求。
	-t 测试所进行的最大秒数。其内部隐含值是-n 50000。它可以使对服务器的测试限制在一个固定的总时间以内。默认时，没有时间限制。
	-p 包含了需要POST的数据的文件.
	-T POST数据所使用的Content-type头信息。
	-v 设置显示信息的详细程度 – 4或更大值会显示头信息， 3或更大值可以显示响应代码(404, 200等), 2或更大值可以显示警告和其他信息。 -V 显示版本号并退出。
	-w 以HTML表的格式输出结果。默认时，它是白色背景的两列宽度的一张表。
	-i 执行HEAD请求，而不是GET。
	-C -C cookie-name=value 对请求附加一个Cookie:行。 其典型形式是name=value的一个参数对。此参数可以重复。
