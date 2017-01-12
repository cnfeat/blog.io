---
layout: post
title: MySQL 新建数据库并导入数据流程
date: 2017-01-12
categories: blog
tags: [mysql]
description: MySQL 新建数据库并导入数据流程
---

环境：windows系统下，WAMP（win+apache+mysql+php）一键安装包

### 一、打开命令行提示符，并切换到mysql文件夹的bin目录下

1. win+r 打开运行框，输入cmd，回车
2. 在输入行下输入：cd  /d  E:\AppServ\MySQL\bin

### 二、命令行下登录mysql数据库（按照提示输入root用户的登录密码）

    E:\AppServ\MySQL\bin>  mysql -uroot -p

### 三、先查看已有的数据库：

    mysql>  show databases;

### 四、新建一个数据库：

    mysql>  create database dbname;

### 五、切换到新建的数据库，并导入数据：

    mysql>  use dbname;
    mysql>  set names utf8;
    mysql>  source E:\dbcontent.sql;

### 六、新建一个用户，专门用来访问此数据库，授权用户拥有此数据库的所有权限（某个数据库的所有权限）：

    mysql>  GRANT all privileges ON dbname.* TO username@localhost IDENTIFIED BY  'password'; 
    mysql>  FLUSH PRIVILEGES;   //刷新系统权限表

### 七、退出root用户，并用新建的帐号username登录测试：

    mysql> exit;
    E:\AppServ\MySQL\bin>  mysql -uusername -p

### 八、能正常登录，则新建成功。流程结束。
 
 