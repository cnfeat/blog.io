---
layout: post
title: scp命令详解
date: 2017-03-17
categories: blog
tags: [ssh]
description: scp命令详解
---

### 不同的Linux之间copy文件常用有3种方法：

第一种就是ftp，也就是其中一台Linux安装ftp Server，这样可以另外一台使用ftp的client程序来进行文件的copy。

第二种方法就是采用samba服务，类似Windows文件共享，用 copy 的方式来操作，比较简洁方便。

第三种就是利用scp命令来进行文件复制。

> scp是有Security的文件copy，基于ssh登录。操作起来比较方便。可以复制文件，也可以复制文件夹。

### 比如要把当前一个文件copy到远程另外一台主机上，可以如下命令。

	scp /home/open/tools.tar.gz root@xxx.xxx.xxx.xxx:/home/root

然后会提示你输入另外那台xxx.xxx.xxx.xxx主机的root用户的登录密码，接着就开始copy了。

### 如果想反过来操作，把文件从远程主机copy到当前系统，也很简单。

	scp root@xxx.xxx.xxx.xxx:/home/root/tools.tar.gz home/open/tools.tar.gz

### 命令基本格式： 

       scp [可选参数] file_source file_target 

### 可能有用的几个参数 : 

	-v 用来显示进度。可以用来查看连接，认证，或是配置错误
	-C 使能压缩选项
	-P 选择端口。注意 -p 已经被 rcp 使用
	-4 强行使用 IPV4 地址
	-6 强行使用 IPV6 地址
 
### 注意两点：

1.如果远程服务器防火墙有特殊限制，scp便要走特殊端口，命令格式如下：
 
	scp -P 9922 remote@www.AAA.com:/usr/local/sin.sh /home/administrator

2.使用 scp 要注意所使用的用户是否具有可读取远程服务器相应文件的权限。