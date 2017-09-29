---
layout: post
title: 查看 MYSQL 数据库中所有用户及拥有权限
date: 2016-12-23
categories: blog
tags: [mysql]
description: 查看 MYSQL 数据库中所有用户及拥有权限
---

### 查看MYSQL数据库中所有用户

`mysql> SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;`

### 查看数据库中具体某个用户的权限

方法一：

`mysql> show grants for 'cactiuser'@'%';`    
 
方法二：

`mysql> select * from mysql.user where user='cactiuser' \G`

### 查看user表结构，需要具体的项可结合表结构来查询

`mysql> desc mysql.user;`