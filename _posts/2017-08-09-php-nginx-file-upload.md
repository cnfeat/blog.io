---
layout: post
title: PHP和Nginx文件上传大小限制问题解决方法
date: 2017-08-09
categories: blog
tags: [nginx,php]
description: PHP和Nginx文件上传大小限制问题解决方法
---

对于nginx+php的一些网站，上传文件大小会受到多个方面的限制：

一个是nginx本身的限制，限制了客户端上传文件的大小

一个是php.ini文件中默认了多个地方的设置。

所以为了解决上传文件大小限定的问题必须要做出多处修改。整理如下：

### 一、修改 `/usr/local/nginx/conf/nginx.conf` 文件

查找 `client_max_body_size` 将后面的值设置为你想设置的值。比如：

	# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
	location ~ \.php$ {
		root          /home/www/htdocs;
		fastcgi_pass  127.0.0.1:9000;
		fastcgi_index index.php;
		fastcgi_param SCRIPT_FILENAME /home/www/htdocs$fastcgi_script_name;
		include        fastcgi_params;

		client_max_body_size 35m;        #客户端上传文件大小设为35M
		client_body_temp_path /home/www/nginx_temp;        #设置临时目录
	}

### 二、修改php.ini

在 `php.ini` 里面查看如下行：

	upload_max_filesize = 8M
	post_max_size = 10M
	memory_limit = 20M
	max_execution_time=300
	file_uploads = On    # 默认允许HTTP文件上传，此选项不能设置为OFF。

	upload_tmp_dir =/tmp/www

在上传大文件时，你会有上传速度慢的感觉，当超过一定的时间，会报脚本执行超过 30秒 的错误。

这是因为在 `php.ini` 配置文件中 `max_execution_time` 配置选项在作怪，其表示每个脚本最大允许执行时间(秒)，`0` 表示没有限制。

你可以适当调整 `max_execution_time` 的值，如设为`300`（秒），不推荐设定为 `0`。
