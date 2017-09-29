---
layout: post
title: Windows系统下单网卡设置双IP实现访问不同网段的方法
date: 2017-04-01
categories: blog
tags: [windows]
description: Windows系统下单网卡设置双IP实现访问不同网段的方法
---

在Windows系统下即使只有一块网卡，同样可以实现双IP访问不同网段。方法如下：

例如：

外网信息：

	IP:192.168.100.2
	子网掩码:255.255.255.0
	网关：192.168.100.1

内网信息：

	IP:192.168.200.123
	子网掩码:255.255.255.0
	网关：192.168.200.1

具体实现步骤如下面两图：

![](https://azraelgreen.github.io/img/20170401_win_1.png)

![](https://azraelgreen.github.io/img/20170401_win_2.png)

设置完毕后即可用一个网卡同时访问两个不同的网段了！

### 备注：

单网卡双线路（如电信、联通），其中网关只能放一个！

点击确定后直接生效，不需要重启系统。