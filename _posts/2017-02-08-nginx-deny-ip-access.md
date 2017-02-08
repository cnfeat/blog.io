---
layout: post
title: nginx禁止IP访问网站设置方法
date: 2017-02-08
categories: blog
tags: [nginx]
description: nginx禁止IP访问网站设置方法
---

为什么要禁止ip访问页面呢，这样做是为了避免其他人把未备案的域名解析到自己的服务器IP，而导致服务器被断网，我们可以通过禁止使用ip访问的方法，防止此类事情的发生。

Nginx的默认虚拟主机在用户通过IP访问，或者通过未设置的域名访问(比如有人把他自己的域名指向了你的ip)的时候生效。

最关键的一点是，在server的设置里面添加这一行：

`listen 80 default;`

后面的default参数表示这个是默认虚拟主机。

这个设置非常有用。

比如别人通过ip或者未知域名访问你的网站的时候，你希望禁止显示任何有效内容，可以给他返回500。

网站主关闭空主机头，防止未备案的域名指向过来造成麻烦。就可以这样设置：

    server { 
		listen 80 default; 
		return 500; 
	} 

也可以把这些流量收集起来，导入到自己的网站，只要做以下跳转设置就可以：

	server { 
		listen 80 default; 
		rewrite ^(.*) http://www.test.com permanent; 
	} 

按照如上设置后，确实不能通过IP访问服务器了。但是当 `server_name` 后跟多个域名时，其中一个域名会无法访问！

举例：没更改之前，通过 `server_name` 中的 `www.test.com test.com` 均可访问服务器，加入禁止IP访问的设置后，通过 `test.com` 无法访问服务器了，`www.test.com` 可以访问。

用 `nginx -t` 检测配置文件会提示warning。

### 解决方法：

在 `listen 80 default;` 后再加 `server_name _;` 

	#禁止IP访问
	server{ 
		listen 80 default; 
		server_name _; 
		return 500; 
	} 

或者

	server { 
		listen 80 dufault; 
		server_name _; 
		rewrite ^(.*) http://www.test.com permanent; 
	} 

这样，通过 `test.com` 就能访问服务器了，问题解决。