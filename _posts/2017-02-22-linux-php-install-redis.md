---
layout: post
title: Linux下php安装Redis扩展
date: 2017-02-22
categories: blog
tags: [php]
description: Linux下php安装Redis扩展
---

### 说明：

操作系统：CentOS

php安装目录：`/usr/local/php`

php.ini配置文件路径：`/usr/local/php/etc/php.ini`

Nginx安装目录：`/usr/local/nginx`

Nginx网站根目录：`/usr/local/nginx/html`



### 1、安装编译工具

`yum install wget  make gcc gcc-c++ zlib-devel openssl openssl-devel pcre-devel kernel keyutils  patch perl`

### 2、安装redis

下载：[https://github.com/nicolasff/phpredis/archive/2.2.4.tar.gz](https://github.com/nicolasff/phpredis/archive/2.2.4.tar.gz)

上传 phpredis-2.2.4.tar.gz 到 `/usr/local/src`目录

	cd /usr/local/src #进入软件包存放目录
	tar zxvf phpredis-2.2.4.tar.gz #解压
	cd phpredis-2.2.4 #进入安装目录
	/usr/local/php/bin/phpize #用phpize生成configure配置文件
	./configure --with-php-config=/usr/local/php/bin/php-config  #配置
	make  #编译
	make install  #安装

安装完成之后，出现下面的安装路径

`/usr/local/php/lib/php/extensions/no-debug-non-zts-20090626/`

### 3、配置php支持

    vi /usr/local/php/etc/php.ini  #编辑配置文件，在最后一行添加以下内容

添加

	extension="redis.so"

	:wq! #保存退出

### 4、测试

	vi /usr/local/nginx/html/index.php   #编辑

	<?php
		phpinfo();
	?>

	:wq! #保存退出

浏览器打开 index.php，可以看到redis相关信息

至此，Linux下php安装redis完成！

### 备注：

如果提示找不到 phpize，需要安装以下软件包：

`yum install php-devel  `