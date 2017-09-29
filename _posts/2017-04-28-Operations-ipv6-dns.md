---
layout: post
title: 通过IPV6地址访问域名，如何设置域名解析
date: 2017-04-28
categories: blog
tags: [operations]
description: 通过IPV6地址访问域名，如何设置域名解析
---

当您希望访问者通过 IPv6 地址访问您的域名时，您可以**使用 AAAA 记录设置域名解析**。

设置方法如下：

![](https://azraelgreen.github.io/img/20170428_ipv6_dns.png)

	A. 记录类型：选 AAAA

	B. 主机记录：填写子域名。
	若要将域名解析为 www.example.com，在主机记录填写 www；
	若要将域名解析为 example.com（不带www），在主机记录填写 @ 或者不填写。

	C.解析线路：若您未设置特定解析线路，则所有线路用户均访问该目标地址；
	若设置了特定解析线路（例如：联通），则特定线路用户访问特定目标地址，其他线路用户仍然访问该（默认）目标地址。

	D.记录值：为 IP 地址，且 AAAA 记录值只可以填写 IPv6 地址。

	E.TTL：默认（10 分钟）即可。