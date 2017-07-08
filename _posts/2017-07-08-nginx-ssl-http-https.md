---
layout: post
title: 解决nginx配置ssl允许http https同时访问的方法
date: 2017-07-08
categories: blog
tags: [nginx]
description: 解决nginx配置ssl允许http https同时访问的方法
---

在配置完成https之后，可能有的站长也想支持http访问。既然有需求那么我们看看怎么解决nginx配置ssl允许http https同时访问的方法。

如果在配置https之后，不通过301重定向的到https的话，那么在访问http的时候就会出现报错。

	400 Bad Request
	The plain HTTP requset was sent to HTTPS port. Sorry for the inconvenience.
	Please report this message and include the following information to us.
	hank you very much!

当然，你要是想同时支持http和https访问的话，301重定向肯定是没得做了。不多说，我们看看怎么操作。

实例：假设我们的conf配置如下。


	1. server {
	2.     server_name YOUR_DOMAINNAME_HERE;
	3.     listen 443;
	4.     ssl on;
	5.     ssl_certificate /usr/local/nginx/conf/server.crt;
	6.     ssl_certificate_key /usr/local/nginx/conf/server.key;
	7. }

编辑之后：


	1. server {
	2.     server_name YOUR_DOMAINNAME_HERE;
	3.     listen 443 ssl;
	4.     ssl_certificate /usr/local/nginx/conf/server.crt;
	5.     ssl_certificate_key /usr/local/nginx/conf/server.key;
	6. }

把 `ssl on;` 这行去掉，ssl 写在 443 端口后面。

这样 http 和 https 都可以访问，完美解决 nginx 配置 ssl 允许 http https同时访问的问题。
