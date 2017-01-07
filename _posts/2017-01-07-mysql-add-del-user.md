---
layout: post
title: Mysql创建、删除用户
date: 2017-01-07
categories: blog
tags: [mysql]
description: Mysql创建、删除用户
---

### MySql中添加用户，新建数据库，用户授权，删除用户，修改密码 

(注意每行后边都跟个;表示一个命令语句结束)

### 一、新建用户

登录MYSQL：

    @> mysql -u root -p

    @> 密码

创建用户：

    mysql> insert into mysql.user(Host,User,Password) values("localhost","test",password("1234"));

这样就创建了一个名为：test 密码为：1234 的用户。

注意：此处的"localhost"，是指该用户只能在本地登录，不能在另外一台机器上远程登录。** 如果想远程登录的话，将"localhost"改为"%" **，表示在任何一台电脑上都可以登录。也可以指定某台机器可以远程登录。

然后登录一下：

    mysql> exit;

    @> mysql -u test -p

    @> 输入密码

    mysql> 登录成功

### 二、修改指定用户密码

    mysql> update mysql.user set password=password('新密码') where User="test" and Host="localhost";

    mysql> flush privileges;

### 三、为用户授权

授权格式：`grant 权限 on 数据库.* to 用户名@登录主机 identified by "密码";　`

登录MYSQL（有ROOT权限），这里以ROOT身份登录：

    @> mysql -u root -p

    @>密码

首先为用户创建一个数据库(testDB)：

    mysql> create database testDB;

#### 1. 授权test用户拥有testDB数据库的所有权限（某个数据库的所有权限）：

    mysql> grant all privileges on testDB.* to test@localhost identified by '1234';

    mysql> flush privileges;  //刷新系统权限表

#### 2. 如果想指定部分权限给一用户，可以这样来写:

    mysql> grant select,update on testDB.* to test@localhost identified by '1234';

    mysql> flush privileges; //刷新系统权限表

授权test用户拥有所有数据库的某些权限： 　
 
    mysql> grant select,delete,update,create,drop on *.* to test@"%" identified by "1234";

test用户对所有数据库都有select,delete,update,create,drop 权限。

@"%" 表示对所有非本地主机授权，不包括localhost。

对localhost授权：

    grant all privileges on testDB.* to test@localhost identified by '1234';

### 四、删除用户

    mysql> Delete FROM user Where User='test' and Host='localhost';

    mysql> flush privileges;

    mysql> drop database testDB;   //删除用户的数据库

删除账户及权限：

    mysql> drop user 用户名@'%';

    mysql> drop user 用户名@localhost; 

查看当前用户（自己）权限：

    show grants;
 
查看其他 MySQL 用户权限：

    show grants for dba@localhost;
 

### 五、撤销已经赋予给 MySQL 用户权限的权限。

revoke 跟 grant 的语法差不多，只需要把关键字 “to” 换成 “from” 即可：

    grant  all on *.* to   dba@localhost;
    revoke all on *.* from dba@localhost;
 
#### MySQL grant、revoke 用户权限注意事项：

1.grant, revoke 用户权限后，该用户只有重新连接 MySQL 数据库，权限才能生效。

2.如果想让授权的用户，也可以将这些权限 grant 给其他用户，需要选项 “grant option“

    grant select on testdb.* to dba@localhost with grant option;

这个特性一般用不到。实际中，数据库权限最好由 DBA 来统一管理。

### 六、其他常用命令：

1.列出所有数据库

    mysql> show database;

2.切换数据库

    mysql> use '数据库名';

3.列出所有表

    mysql> show tables;

4.显示数据表结构

    mysql> describe 表名;

5.删除数据库和数据表

    mysql> drop database 数据库名;

    mysql> drop table 数据表名;