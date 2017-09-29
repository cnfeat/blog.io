---
layout: post
title: centos 7 安装 nginx 的两种方法
date: 2017-08-15
categories: blog
tags: [nginx,centos]
description: centos 7 安装 nginx 的两种方法
---

## 第一种方式：通过yum安装

直接通过 `yum install nginx` 肯定是不行的，因为 yum 没有 nginx，所以首先把 nginx 的源加入 yum 中。

运行下面的命令:

### 1.将nginx放到yum repro库中

	[root@localhost ~]# rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

### 2.查看nginx信息

	[root@localhost ~]# yum info nginx

### 3.使用yum安装ngnix

	[root@localhost ~]# yum install nginx

### 4.启动nginx

	[root@localhost ~]# service nginx start

或：

	systemctl start  nginx.service

### 5.查看nginx版本

	[root@localhost ~]# nginx -v

### 6.访问nginx，现在你可以通过公网ip (本地可以通过 localhost /或 127.0.0.1 ) 查看nginx 服务返回的信息。

	[root@localhost ~]# curl -i localhost

### 7.nginx配置文件位置在/etc/nginx/

	[root@localhost /]# ll /etc/nginx/

### 8.nginx常用操作

启动:

	$ service nginx start

	systemctl start nginx.service（CentOS 7）

重启：

	$ /usr/sbin/nginx –s reload

停止：

	$ /usr/sbin/nginx –s stop

**测试配置文件是否正常：**

	$ /usr/sbin/nginx –t

## 第二种方式：通过手动下载安装包解压安装

### 1.下载nginx包

	[root@localhost ~]# wget http://nginx.org/download/nginx-1.10.1.tar.gz

### 2.复制包到你的安装目录

	[root@localhost ~]# cp nginx-1.10.1.tar.gz /usr/local/

### 3.解压

	[root@localhost ~]# tar -zxvf nginx-1.10.1.tar.gz
	[root@localhost ~]# cd nginx-1.10.1

### 4.启动nginx

	[root@localhost ~]# /usr/local/nginx/sbin/nginx

### 5.查看版本

	[root@localhost ~]# nginx -v

### 6.url访问 nginx localhost 或 127.0.0.1
