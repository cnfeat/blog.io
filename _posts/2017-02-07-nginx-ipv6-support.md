---
layout: post
title: nginx开启IPV6支持配置
date: 2017-02-07
categories: blog
tags: [nginx]
description: nginx开启IPV6支持配置
---

从Nginx 1.3 的某个版本起，默认ipv6only是打开的，也就是上面的语句只会监听IPv6的端口而不会监听IPv4的端口。虽然Linux系统默认是监听IPv6的某个端口会同时监听对应的IPv4的端口，但是FreeBSD是默认分开IPv6和IPv4的。所以为了一致性的考虑（新版本Nginx必须推荐这样做），请使用分开监听的方法：

    listen 80;
    listen [::]:80 ipv6only=on;

IPV4日益稀缺，ipv6已经慢慢走上日程，待ipv6在国内普及，使用nginx配置ipv6那是肯定的，看看如何让nginx支持ipv6以及配置。

### 查看nginx是否支持ipv6

    # /usr/local/nginx-1.7.0/sbin/nginx -V
	nginx version: nginx/1.7.0
	built by gcc 4.4.7 20120313 (Red Hat 4.4.7-3) (GCC) 
	configure arguments: --prefix=/usr/local/nginx-1.7.0 --with-http_stub_status_module

没有出现--with-ipv6,说明当前的nginx不支持ipv6，所以我们需要重新编译nginx，配置里面增加--with-ipv6，具体怎么安装，我不在啰嗦了。

### 同时监听IPV4和IPV6

	server {
		....
		listen [::]:80;
		...
	}
 
### 只监听IPV6

	server {
		....
		listen [::]:80 default ipv6only=on;
		...
	}

### 监听指定IPV6地址

	server {
		....
		listen [3608:f0f0:3002:31::1]:80;
		...
	}

### 重启nginx

    /usr/local/nginx-1.7.0/sbin/nginx -s reload
 