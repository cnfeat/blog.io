---
layout: post
title: 如何查看php版本
date: 2017-02-15
categories: blog
tags: [php]
description: 如何查看php版本
---

### 命令行查询

如果已经配置好环境变量，直接在命令行中输入

`php -v`

将会显示php的版本信息。

如果没有配置环境变量，直接在命令行中进入到php的安装目录后，再输入命令

`php -v`

### 使用phpinfo()函数查询

新建一个php文件，在文件中输入

	<?php
	     echo phpinfo();
	?>

访问这个网页就会显示详细的php配置信息。