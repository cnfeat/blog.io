---
layout: post
title: Ubuntu 系统下架设个人私有云 owncloud 服务
date: 2016-09-26
categories: blog
tags: [ubuntu]
description: Ubuntu 系统下架设个人私有云 owncloud 服务

---

系统：Ubuntu_16.04 

软件：owncloud-9.1.1-1.1

### 最简单的方法是通过源安装，首先导入key：

    wget -nv https://download.owncloud.org/download/repositories/stable/Ubuntu_16.04/Release.key -O Release.key
    apt-key add - < Release.key

### 然后运行以下安装命令：

    sh -c "echo 'deb http://download.owncloud.org/download/repositories/stable/Ubuntu_16.04/ /' >> /etc/apt/sources.list.d/owncloud.list"
    apt-get update
    apt-get install owncloud

直接下载安装包：
[Direct Download](http://download.owncloud.org/download/repositories/stable/Ubuntu_16.04/)


### 备注：

默认安装后，owncloud服务器所在路径：/var/www/owncloud

以上目录下的 data 文件夹，即为保存的用户数据

网页访问路径：http://服务器ip地址/owncloud/

手机、电脑均可下载安装对应客户端，使用非常方便！

owncloud官方网站下载：[https://download.owncloud.org](https://download.owncloud.org)