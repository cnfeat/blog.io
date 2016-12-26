---
layout: post
title: 查看MySQL的错误日志的方法
date: 2016-12-26
categories: blog
tags: [mysql]
description: 查看MySQL的错误日志的方法
---

MySQL的错误信息是在data目录下，且文件名为

     <hostname>.err (<hostname>指的是主机名)

但由于每个人安装的环境不一样，或你忘记了data目录的所在位置，你可以通过下面方法查找：

一、查看 `/etc/my.cnf` 配置文件里的数据目录：

    vi /etc/my.cnf
    datadir=/var/lib/mysql

    cd /var/lib/mysql
    vi web.localdomain.err

二、通过主机名查找：

    #hostname //获得主机名 
    <hostname> 

    #find / -name <hostname>.err 
    #cd ... 
    #vi <hostname>.err