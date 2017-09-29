---
layout: post
title: Windows和Linux下定时删除某天前的文件的脚本
date: 2017-05-09
categories: blog
tags: [linux,windows]
description: Windows和Linux下定时删除某天前的文件的脚本
---

### 一、windows删除脚本（测试可用）

删除 N 天之前的文件脚本：`cleardbbak.bat`

	forfiles /p E:\db_backup /m * /d -10 /c "cmd /c del @file"
	forfiles /p E:\db_backup /m * /d -10 /c "cmd /c del @file"

将上面的目录换成指定的目录，

`*` 可以筛选文件格式，

`/d` 后面的参数：为负数表示多少天之前的，正数是多少天之后的。

保存成 `bat` 文件，然后在 Windows计划任务 里面设置定时执行的时间就可以了。

### 二、linux删除脚本

`vi dbclear.sh`

#### 写法1：

	#!/bin/bash
	/usr/bin/find /dbbak -mtime +10 -name "*.dmp" -exec rm -rf {} \;
	/usr/bin/find /dbbak -mtime +10 -name "*.log" -exec rm -rf {} \;

#### 写法2:

	#!/bin/bash
	# 删除30天之前的文件
	find /var/usr/nginx/logs/ -mtime +30 -type f -name \*.gz | xargs rm -f

以上两种写法都可以。

将上面的目录换成自己指定的目录，

后面的 `\*.gz` 表示文件扩展名，

`-mtime` 后面的参数与上面 Windows 的相反，正数表示多少天之前的文件。

将上面的内容保存成 `.sh` 并使用 `chmod +x` 设置成可执行权限，然后放到定时任务中去执行即可。

#### 设定计划任务：

	crontab -e

	# 每天的 01:01 执行后面的角本
	1   1     *  *  *   /root/system/dbclear.sh