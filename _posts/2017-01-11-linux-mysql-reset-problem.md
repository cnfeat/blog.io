---
layout: post
title: MySQL root密码重置报错
date: 2017-01-11
categories: blog
tags: [mysql]
description: MySQL root密码重置报错
---

## MySQL root密码重置报错：mysqladmin: connect to server at 'localhost' failed的解决方案
 
### 1  登陆失败,mysqladmin修改密码失败

    [root@mysql var]# mysqladmin -u root password '123456'

    mysqladmin: connect to server at 'localhost' failed
    error: 'Access denied for user 'root'@'localhost' (using password: NO)'
 
### 2 停止mysql服务

    [root@mysql var]# /etc/init.d/mysqld stop

    Shutting down MySQL.... SUCCESS!
 
### 3 安全模式启动

    [root@mysql var]# mysqld_safe --skip-grant-tables &
    /opt/mysql/product/5.5.25a/bin/mysqld_safe --skip-grant-tables &

    [1] 10912
 
### 4 无密码root帐号登陆

    [root@mysql var]# /usr/bin/mysql -uroot -p 
    【注释，在下面的要求你输入密码的时候，你不用管，直接回车键一敲就过去了】

    Enter password:
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 48
    Server version: 5.1.41-log Source distribution
 
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  
    mysql> use mysql;
    Database changed
 
### 5 手动update修改密码

    mysql> update user set password=password("guxxxxxahyVh") where user='root' and host='localhost';

    Query OK, 1 row affected (0.00 sec)
    Rows matched: 1  Changed: 1  Warnings: 0
 
    mysql> flush privileges;
    Query OK, 0 rows affected (0.00 sec)
 
    mysql> quit
    Bye
 
### 6 正常重新启动

    [root@mysql var]# service mysqld restart
 
### 7 其他形式的错误情况分析

#### 7.1 找不到sock 报错 ：

    [root@app60 mysqld]#  /usr/bin/mysql -uroot -p

    Enter password:
    ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (111)
    [root@app60 mysqld]#
 
登陆的时候加上sock参数就OK了。

    [root@app60 mysqld]#  /usr/bin/mysql -uroot -p --socket=/opt/mysqldata/mysql.sock

    Enter password:
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 6
    Server version: 5.1.69 Source distribution
 
#### 7.2 抱错 [ERROR] /usr/libexec/mysqld: Error writing file '/var/run/mysqld/mysqld.pid' (Errcode: 28)
 
[分析]：不能写入默认的pid文件，就 修改 /etc/init.d/mysqld，把pid指向别的路经
 
    [root@app60 mysqld]# vi /etc/init.d/mysqld
    .....
    get_mysql_option mysqld datadir "/var/lib/mysql"
    datadir="$result"
    get_mysql_option mysqld socket "$datadir/mysql.sock"
    socketfile="$result"
    get_mysql_option mysqld_safe log-error "/var/log/mysqld.log"
    errlogfile="$result"
    get_mysql_option mysqld_safe pid-file "/opt/mysqldata/mysqld.pid"    # '/var/run/mysqld/mysqld.pid' 原始值,这里修改成别的路径/opt/mysqldata/mysqld.pid
    mypidfile="$result"
    .....

然后启动mysqld服务，OK，成功了。
 
 