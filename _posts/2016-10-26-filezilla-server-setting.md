---
layout: post
title: filezilla server 虚拟目录配置
date: 2016-10-26
categories: blog
tags: [centos,ftp]
description: filezilla server 虚拟目录配置

---

FileZilla Server 的虚拟目录设置与其它 FTP 服务器软件有所不同。

在FileZilla Server中设置虚拟目录，必须采用 **FTP根目录+虚拟目录名** 的形式来命名 alias 别名。

**添加Aliases别名，注意一定用“ /”符号开头，表示是根目录下的虚拟目录。**
 
### 举例：

FileZilla Server的根目录(即Home目录)设为 D:\FTP\，

现在要将 D:\softs 加入虚拟目录，

则第一步添加新目录 D:\softs ;

第二步在已添加目录项 D:\softs 单击右键，

选择 edit alias(别名)，

设置 alias为 /softs (alias-name)。
 
其中 alias-name 为虚拟目录名，可以与实际的目录名不同。