---
layout: post
title: phpmyadmin打开空白或无法登录
date: 2017-01-04
categories: blog
tags: [mysql]
description: phpmyadmin打开空白或无法登录
---

处理方法：

`vi /etc/php.ini`

找到 `open_basedir = .:/tmp/`

修改为

`open_basedir = none`

如果还是无法登录 phpMyAdmin，

可能是因为 /var/lib/php/session 的权限还是“apache”(如果安装的是Nginx环境的话)。

查看：

`ll /var/lib/php`

如果是 apache，修改:

`chown nginx.nginx /var/lib/php/session`