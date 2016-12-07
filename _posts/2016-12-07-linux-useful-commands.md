---
layout: post
title: Linux 常用命令
date: 2016-12-07
categories: blog
tags: [linux]
description: Linux 常用命令
---

## diff 用法：

语法：`diff file1 file2`

以小于号( < )开头的那行是仅在 file1 中发现有的

用大于号( > )开头的那行是仅在 file2 中发现有的

没有显示出来的是两个文件都有的

## grep命令

语法：`grep [选项] pattern [文件名]`

参数"`-v`", 会显示出所有不包含匹配文本的内容

利用“`-i`”，搜索的时候会忽略大小写

利用“`-r`”, 来在所有的子目录下执行相应的查找

`grep John /etc/passwd`

## find命令

`语法: find 路径 约束条件`

查找在“ /etc ”目录下所有文件名中含有“ mail ”的文件

`find /etc -name "*mail*"`

会列出系统中所有大于 100M 的文件

`find / -type f -size +100M`

列出在当前目录下在最近 60天 没有被修改过文件

`find . -mtime +60`

在当前目录下在最近 2天 被修改过文件

`find . –mtime -2`

## xargs命令

以取一个命令的输出作为另一个命令的参数

获得 /etc 下所有以 .conf 结尾的文件。可以有多种方法获得如下结果。

以下命令仅仅为了帮助大家理解如何使用 xargs.find 命令的输入结果一个接一个的传递给 xargs，作为 `ls -l` 的参数。

`find /etc -name "*.conf" | xargs ls –l `