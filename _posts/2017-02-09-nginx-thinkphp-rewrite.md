---
layout: post
title: Nginx部署ThinkPHP项目伪静态配置
date: 2017-02-09
categories: blog
tags: [nginx]
description: Nginx部署ThinkPHP项目伪静态配置
---

配置文件：`/etc/nginx/conf.d/default.conf`

用的阿里云服务器的lnmp一件安装包

配置为：

	location / {
    	if (!-e $request_filename) {
        	rewrite ^(.*)$ /index.php?s=$1 last;
     		break;
    	}
	}