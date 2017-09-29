---
layout: post
title: centos下查看mysql版本方法
date: 2017-01-16
categories: blog
tags: [mysql]
description: centos下查看mysql版本方法
---

CentOS系统下查看MySQL版本有多种方法：

一、在mysql中： `mysql> status;`

    --------------
    mysql  Ver 14.14 Distrib 5.1.73, for redhat-linux-gnu (x86_64) using readline 5.1

    Connection id:		2260
    Current database:	
    Current user:		root@localhost
    SSL:			Not in use
    Current pager:		stdout
    Using outfile:		''
    Using delimiter:	;
    Server version:		5.1.73-log Source distribution
    Protocol version:	10
    Connection:		Localhost via UNIX socket
    Server characterset:	latin1
    Db     characterset:	latin1
    Client characterset:	latin1
    Conn.  characterset:	latin1
    UNIX socket:		/www/data/mysql/mysql.sock
    Uptime:			56 days 21 hours 26 min 13 sec

二、在mysql中：`mysql> select version();`

    +------------+
    | version()  |
    +------------+
    | 5.1.73-log |
    +------------+
    1 row in set (0.00 sec)

三、命令行下：`mysql --help | grep Distrib`

    mysql  Ver 14.14 Distrib 5.1.73, for redhat-linux-gnu (x86_64) using readline 5.1
