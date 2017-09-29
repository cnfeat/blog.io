---
layout: post
title: CentOS下使用tcpdump网络抓包
date: 2017-06-02
categories: blog
tags: [operations,centos]
description: CentOS下使用tcpdump网络抓包
---

CentOS系统网络抓包用 `tcpdump`，首先当然是看有没有安装了，没有就用 `yum` 安装一下。

`tcpdump` 是 linux 命令行下常用的的一个抓包工具，记录一下平时常用的方式，测试机器系统是centos 7。

### tcpdump的命令格式

tcpdump的参数众多，通过 `man tcpdump` 可以查看 `tcpdump` 的详细说明，这边只列一些常用参数：

	tcpdump [-i 网卡] -nnAX '表达式'

各参数说明如下：

	-i：		interface 监听的网卡。
	-nn：	表示以ip和port的方式显示来源主机和目的主机，而不是用主机名和服务。
	-A：		以ascii的方式显示数据包，抓取web数据时很有用。
	-X：		数据包将会以16进制和ascii的方式显示。
	表达式：	表达式有很多种，常见的有：host 主机；port 端口；src host 发包主机；dst host 收包主机。多个条件可以用and、or组合，取反可以使用!，更多的使用可以查看man 7 pcap-filter。

### 下面进行一些命令测试，如果没有权限，可以先切换成root用户。

#### 监听网卡eth0

	$ tcpdump -i eth0

这个方式最简单了，但是用处不多，因为基本上只能看到数据包的信息刷屏，压根看不清，可以使用ctrl+c中断退出，如果真有需求，可以将输出内容重定向到一个文件，这样也更方便查看。

#### 监听指定协议的数据

	$ tcpdump -i eth0 -nn 'icmp'

这个是用来监听icmp协议的数据，就是ping命令使用的协议。类似的，如果要监听tcp或者是udp协议，只需要修改上例的icmp就可以了。

ping下监听的机器，输出中，每一行的各个数据表示的含义：

	抓到包的时间 IP 发包的主机和端口 > 接收的主机和端口 数据包内容

#### 监听指定的主机

	$ tcpdump -i eth0 -nn 'host 192.168.1.231'

这样的话，192.168.1.231这台主机接收到的包和发送的包都会被抓取。

	$ tcpdump -i eth0 -nn 'src host 192.168.1.231'

这样只有192.168.1.231这台主机发送的包才会被抓取。

	$ tcpdump -i eth0 -nn 'dst host 192.168.1.231'

这样只有192.168.1.231这台主机接收到的包才会被抓取。

#### 监听指定端口

	$ tcpdump -i eth0 -nnA 'port 80'

上例是用来监听主机的80端口收到和发送的所有数据包，结合 `-A` 参数，在web开发中，真是非常有用。

#### 监听指定主机和端口

	$ tcpdump -i eth0 -nnA 'port 80 and src host 192.168.1.231'

多个条件可以用and，or连接。上例表示监听192.168.1.231主机通过80端口发送的数据包。

#### 监听除某个端口外的其它端口

	$ tcpdump -i eth0 -nnA '!port 22'

如果需要排除某个端口或者主机，可以使用“!”符号，上例表示监听非22端口的数据包。

#### 同时可以将数据输出到一个指定的文件

	$ tcpdump -i em2 -vnn port 8000 -w xxx.cap

#### 小结：

tcpdump这个功能参数很多，表达式的选项也非常多，非常强大，不过常用的功能确实不多。详情可以通过man查看系统手册。

另外在抓取web包的时候，发送网页内容都是很奇怪的字符，发现是apache开启了gzip压缩的缘故，关闭掉gzip压缩就可以了。

在centos 7下，编辑 `vim/etc/apache2/mods-enabled/deflate.load` 文件，将加载模块 `deflate_module` 的语句注释掉，然后重启 apache 就OK了。