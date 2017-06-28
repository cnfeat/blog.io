---
layout: post
title: centos下php隐藏http头部版本信息
date: 2017-06-28
categories: blog
tags: [centos,php]
description: centos下php隐藏http头部版本信息
---

## 1、nginx隐藏头部版本信息方法

编辑`nginx.conf`配置文件，在`http{}`内**增加如下一行**

	http {
	      ……
	      server_tokens off;
	      ……
	     }

编辑`php-fpm`配置文件，`fastcgi.conf`或`fcgi.conf`

找到：

	fastcgi_param SERVER_SOFTWARE nginx/$nginx_version;

改为：

	fastcgi_param SERVER_SOFTWARE nginx;

重启nginx服务生效( `nginx -s reload` 重载Nginx配置)

	[root@xmydlinux conf]# curl --head 127.0.0.1                
	HTTP/1.1 200 OK
	Server: nginx

	Content-Type: text/html; charset=utf-8
	Connection: keep-alive
	…………

## 2、apache隐藏头部版本信息

编辑`httpd.conf`文件

找到：

	ServerTokens OS
	ServerSignature On

修改为：

	ServerTokens ProductOnly
	ServerSignature Off

重新启动httpd服务生效

	[root@xmydlinux ~]# curl -I 127.0.0.1             
	HTTP/1.1 200 OK
	Server: Apache

	Accept-Ranges: bytes
	Content-Length: 97
	Connection: close
	Content-Type: text/html

另：可更改源码`include`目录下`ap_release.h`这个文件

	#define AP_SERVER_BASEVENDOR “Apache Software Foundation”  #apache相关字样都可更改
	#define AP_SERVER_BASEPROJECT “Apache HTTP Server”
	#define AP_SERVER_BASEPRODUCT “Apache”

	#define AP_SERVER_MAJORVERSION_NUMBER 2      #版本字段可随意更改
	#define AP_SERVER_MINORVERSION_NUMBER 2
	#define AP_SERVER_PATCHLEVEL_NUMBER 17
	#define AP_SERVER_DEVBUILD_BOOLEAN 0

 
## 3、PHP版本头部文件隐藏返回

修改php.ini文件

找到：

	expose_php = On

修改为：

	expose_php = Off

可以避免http头部信息中返回 “`X-Powered-By: PHP/5.2.17`” 字样。