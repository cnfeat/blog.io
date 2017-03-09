---
layout: post
title: rsync命令排除文件和文件夹
date: 2017-03-09
categories: blog
tags: [rsync]
description: rsync命令排除文件和文件夹
---

假设最开始的命令是这样的 

`rsync -e 'ssh -p 30000' -avl --delete --stats --progress demo@123.45.67.890:/home/demo /backup/ `

### 一、排除单独的文件夹和文件 

要排除sources文件夹，我们可以添加 '--exclude' 选项： 

	--exclude 'sources' 

命令是这样的： 

	rsync -e 'ssh -p 30000' -avl --delete --stats --progress --exclude 'sources' demo@123.45.67.890:/home/demo /backup/ 

要排除 "public_html" 文件夹下的 "database.txt" 文件： 

	--exclude 'public_html/database.txt' 

命令是这样的：
 
	rsync -e 'ssh -p 30000' -avl --delete --stats --progress --exclude 'sources' --exclude 'public_html/database.txt' demo@123.45.67.890:/home/demo /backup/ 

### 二、使用 '--exclude-from' 排除多个文件夹和文件 

建立文件： 

`/home/backup/exclude.txt `

在里面定义要排除的文件夹和文件 

	sources 
	public_html/database.* 
	downloads/test/* 
	uploads 
	download/softs/ 

使用指令： 

	--exclude-from '/home/backup/exclude.txt' 

最后的命令如下： 

	rsync -e 'ssh -p 30000' -avl --delete --stats --progress --exclude-from '/home/backup/exclude.txt' demo@123.45.67.890:/home/demo /backup/