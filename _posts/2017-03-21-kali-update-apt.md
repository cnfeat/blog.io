---
layout: post
title: Kali 2.0替换APT更新源为国内源并更新系统
date: 2017-03-21
categories: blog
tags: [kali]
description: Kali 2.0替换APT更新源为国内源并更新系统
---

修改APT软件源 sources.list：

`vim /etc/apt/sources.list`

可以删除该文件中的所有内容，也可以直接在文前添加新的APT源。

然后选择适合自己较快的源：

	#中科大kali源
	deb http://mirrors.ustc.edu.cn/kali sana main non-free contrib
	deb http://mirrors.ustc.edu.cn/kali-security/ sana/updates main contrib non-free
	deb-src http://mirrors.ustc.edu.cn/kali-security/ sana/updates main contrib non-free

	#阿里云kali源
	deb http://mirrors.aliyun.com/kali sana main non-free contrib
	deb http://mirrors.aliyun.com/kali  sana/updates main contrib non-free
	deb-src http://mirrors.aliyun.com/kali  sana/updates main contrib non-free

对软件进行一次整体更新：

	apt-get update & apt-get upgrade
	apt-get dist-upgrade
	apt-get clean