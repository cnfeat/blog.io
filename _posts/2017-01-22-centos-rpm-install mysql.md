---
layout: post
title: CentOS 6.7 下以RPM方式安装MySQL
date: 2017-01-22
categories: blog
tags: [mysql]
description: CentOS 6.7 下以RPM方式安装MySQL
---

### 首先去 http://downloads.mysql.com/archives/community

分别下载以下三个文件（如果你的机器是64位的请下载64位版本）：

    MySQL-server-5.5.33-1.el6.x86_64.rpm
    http://downloads.mysql.com/archives/get/file/MySQL-server-5.5.33-1.el6.x86_64.rpm

    MySQL-client-5.5.33-1.el6.x86_64.rpm
    http://downloads.mysql.com/archives/get/file/MySQL-client-5.5.33-1.el6.x86_64.rpm

    MySQL-devel-5.5.33-1.el6.x86_64.rpm
    http://downloads.mysql.com/archives/get/file/MySQL-devel-5.5.33-1.el6.x86_64.rpm

可以用 `wget -c 地址` 的方式下载到服务器上

下载完成后开始安装：

`rpm -ivh MySQL-client-5.5.33-1.el6.x86_64.rpm MySQL-server-5.5.33-1.el6.x86_64.rpm MySQL-devel-5.5.33-1.el6.x86_64.rpm`

提示安装完成后，输入mysql 看是否安装成功

`mysql`

如果出现如下错误信息：

    ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)

说明mysql服务还没有启动，输入 `service mysql start` 启动mysql服务

`service mysql start`

然后再输入mysql，若出现以下提示信息，说明成功。

    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 1
    Server version: 5.5.16 MySQL Community Server (GPL)

    Copyright (c) 2000, 2011, Oracle and/or its affiliates. All rights reserved.

    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

### 首次安装时，默认密码为空，可以使用如下命令修改root密码，

`mysqladmin -u root  password mypassword`

mypassword 为你设定的新密码

然后再次登录

`mysql -u root –p`

rpm包安装的MySQL是不会安装 `/etc/my.cnf` 文件的

解决方法，只需要复制/usr/share/mysql目录下的my-huge.cnf 文件到/etc目录，并改名为my.cnf即可

`cp /usr/share/mysql/my-huge.cnf /etc/my.cnf`


配置远程访问

处于安全考虑，Mysql默认是不允许远程访问的，可以使用下面开启远程访问

    //赋予任何主机访问数据的权限
    mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'WITH GRANT OPTION

    //使修改生效
    mysql> FLUSH PRIVILEGES

如果依然不能远程访问的话，那就很可能防火墙的原因了，可以在 **防火墙中开启3306端口** 或者干脆关掉防火墙。 