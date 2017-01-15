---
layout: post
title: centos下完全卸载mysql
date: 2017-01-15
categories: blog
tags: [mysql]
description: centos下完全卸载mysql
---

### yum方式安装的mysql完全卸载:

    yum remove mysql mysql-server mysql-libs compat-mysql51
    rm -rf /var/lib/mysql
    rm /etc/my.cnf

查看是否还有mysql软件：

    rpm -qa|grep mysql

如果存在的话，继续删除即可。

### rpm方式安装的mysql完全卸载:

#### a）查看系统中是否以rpm包安装的mysql：

    [root@localhost opt]# rpm -qa | grep -i mysql
    MySQL-server-5.6.17-1.el6.i686
    MySQL-client-5.6.17-1.el6.i686

#### b)卸载mysql

    [root@localhost local]# rpm -e MySQL-server-5.6.17-1.el6.i686
    [root@localhost local]# rpm -e MySQL-client-5.6.17-1.el6.i686

#### c)删除mysql服务

    [root@localhost local]# chkconfig --list | grep -i mysql
    [root@localhost local]# chkconfig --del mysql

#### d)删除分散mysql文件夹

    [root@localhost local]# whereis mysql 或者 find / -name mysql
    mysql: /usr/lib/mysql /usr/share/mysql

清空相关mysql的所有目录以及文件

    rm -rf /usr/lib/mysql
    rm -rf /usr/share/mysql
    rm -rf /usr/my.cnf

通过以上几步，mysql应该已经完全卸载干净了