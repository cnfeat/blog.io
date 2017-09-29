---
layout: post
title: 使用netstat检测及监测网络连接
date: 2017-05-04
categories: blog
tags: [operations]
description: 使用netstat检测及监测网络连接
---

> 大家都知道，Linux上的web服务每天都要面临成千上万的连接，这些连接都是要遵循TCP协议的。既然都是TCP协议连接，那就不得不面临一个网路最大的安全问题，DOS攻击及DDOS攻击，这些攻击是没有办法抹除的，因为这是针对TCP协议本身的一个设计缺陷儿造成的。所以，这就要求运维人员，时刻监测系统安全，是否处于被DOS攻击状态。

### 那么怎么监测及检测的呢？

这就要用到我一开始就要提到的 `netstat` 命令。

### 先简单的介绍一下 `netstat` 命令的主要作用：

可以查看系统当前的连接状态，不管是TCP连接还是udp协议连接，以及每个连接的进程号、是哪个应用程序、连接所用的端口号，这些都可以陈列出来。是不是很强大。

### TCP连接的状态：

TCP进行3次握手，其过程有很多状态，不同的连接状态，都有想对应的状态码，看下面列表：

	LISTEN：侦听来自远方的TCP端口的连接请求
	SYN-SENT：再发送连接请求后等待匹配的连接请求
	SYN-RECEIVED：再收到和发送一个连接请求后等待对方对连接请求的确认
	ESTABLISHED：代表一个打开的连接
	FIN-WAIT-1：等待远程TCP连接中断请求，或先前的连接中断请求的确认
	FIN-WAIT-2：从远程TCP等待连接中断请求
	CLOSE-WAIT：等待从本地用户发来的连接中断请求
	CLOSING：等待远程TCP对连接中断的确认
	LAST-ACK：等待原来的发向远程TCP的连接中断请求的确认
	TIME-WAIT：等待足够的时间以确保远程TCP接收到连接中断请求的确认
	CLOSED：没有任何连接状态

大家最好一定要记住这些状态，因为运维人员在监控系统并发连接状态时，监控系统返回的也是这些状态码！

### 常用 netstat 命令：

以下这条命令将会显示出netstat的帮助信息，不懂的以及不太了解这个命令有哪些参数可用的都可以在这个命令的返回信息中看到：

	#netstat --help

显示当前所有活动的网络连接：

	#netstat -na

显示出所有处于监听状态的应用程序及进程号和端口号：

	#netstat -aultnp

如果想对一个单一的进行查询，只需要在命令后面再加上“`| grep $`”。这里就用到了管道符，以及 `grep` 筛选命令，`$` 代表参数，也就是你要查询的那个。

如要显示所有80端口的网络连接：

	#netstat -aultnp | grep 80

如果还想对返回的连接列表进行排序，这就要用到 `sort` 命令了，命令如下：

	#netstat -aultnp | grep :80 | sort

当然，如果还想进行统计的话，就可以再往后面加 `wc` 命令。如：

	#netstat -aultnp | grep :80 | wc -l

其实，要想监测出系统连接是否安全，要进行多状态的查询，以及要分析，总结，还有就是经验。总的下来，才可以判断出连接是否处于安全状态。

下面就给大家再举一些例子，让大家彻底的明白，及彻底的理解这个命令的用处，使其发挥出最大功能。

	#netstat -n -p|grep SYN_REC | wc -l

这个命令可以查找出当前服务器有多少个活动的 SYNC_REC 连接。正常来说这个值很小，最好小于5。 当有Dos攻击或者邮件炸弹的时候，这个值相当的高。尽管如此，这个值和系统有很大关系，有的服务器值就很高，也是正常现象。

	#netstat -n -p | grep SYN_REC | sort -u

列出所有连接过的IP地址。

	#netstat -n -p | grep SYN_REC | awk '{print $5}' | awk -F: '{print $1}'

列出所有发送SYN_REC连接节点的IP地址。

	#netstat -ntu | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -n

使用netstat命令计算每个主机连接到本机的连接数。

	#netstat -anp |grep 'tcp|udp' | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -n

列出所有连接到本机的UDP或者TCP连接的IP数量。

	#netstat -ntu | grep ESTAB | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr

检查 ESTABLISHED 连接并且列出每个IP地址的连接数量。

	#netstat -plan|grep :80|awk {'print $5'}|cut -d: -f 1|sort|uniq -c|sort -nk 1

列出所有连接到本机80端口的IP地址和其连接数。80端口一般是用来处理HTTP网页请求。

那么，如果真的发现有大量的假连接了，那么也不要慌，要先找出一些“另类的IP地址”，怎么解释呢，因为在进行Dos攻击时，会为造出大量的假IP去连接服务器，进行3次握手，所以，这就要根据经验去找出假IP，然后通过防火墙规则，添加一个规则拒接这个假IP的网段连接。

例如：

	#iptables -A INPUT 1 -s $IPADRESS -j DROP/REJECT

注意，你需将 `$IPADRESS` 替换成需要拒绝连接的IP地址。执行完 iptables 后呢，要重启一下web服务。