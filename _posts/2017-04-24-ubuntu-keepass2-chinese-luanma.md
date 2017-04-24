---
layout: post
title: keepass2在Ubuntu 16.10下中文乱码的解决办法
date: 2017-04-24
categories: blog
tags: [ubuntu]
description: keepass2在Ubuntu 16.10下中文乱码的解决办法
---

### 问题：

UBUNTU 16.10 下 keepass2 菜单和对话框中文乱码

### 原因：

经检查发现是 **默认Ubuntu字体族未映射中文字体**

### 解决办法：

修改 `65-nonlatin.conf` 配置文件 ：

	sudo vi /etc/fonts/conf.avail/65-nonlatin.conf

增加如下内容即可：

	<alias>
	<family>Ubuntu</family>
	<prefer>
	<family>sans-serif</family>
	</prefer>
	</alias>

### 备注：

通过在运行命令前，添加 `FC_DEBUG=1` ，可以查找程序用到的字体。可解决因缺少中文支持导致乱码的问题。