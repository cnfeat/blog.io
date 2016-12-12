---
layout: post
title: CentOS 下 mysql 数据库备份与还原
date: 2016-12-12
categories: blog
tags: [centos,mysql]
description: CentOS 下 mysql 数据库备份与还原
---

## 一、数据库的备份

### 1.备份整个数据库：

进入 MySql 下的 Bin 目录，如：`cd /usr/local/mysql/bin`
 
基本备份

    mysqldump -u用户 -p密码 数据库名 > （目录）导出文件名
    mysqldump --default-character-set=utf8 -uroot -ppassword mydb > backup.sql

注1：

mydb就是要备份的数据库的名称。

上例中数据库的用户名为 root，密码是 password;

备份结果保存在当前目录下 backup.sql 中

注2：

如果密码里有！等特殊字符，需要用单引号把密码括起来！

### 2.备份数据库中的一个表

导出一个表(包括表的数据）：

    mysqldump -u 用户名 -p 数据库名.表名> 导出的文件名
    mysqldump -uroot -ppassword mydb.mytable > backup.sql
 
### 3.mysqldump 远程备份(整个数据库)

    mysqldump -h ip -uroot -ppassword mydb > backup.sql
    mysqldump -h 192.168.0.254 -uroot -ppassword mydb > backup.sql

## 二、数据库的还原

对于文件的导入：

在 Centos 下首先要新建一个(同名的)数据库。

例如：

          mysql> create database mydb;
          mysql> use mydb;
          mysql> set names utf8;
 
          导入数据库.sql文件的位置
          mysql> source /home/azrael/backup.sql;
 
接着你会看到屏幕上很多查询语句的成功，然后就OK了。

### 备注：

以上还原数据库操作是在登录 mysql 帐号后执行。其实也可以不登录 mysql，直接在命令行下执行还原操作：

### shell脚本中执行mysql语句的方法

第一种是使用“ -e ”参数来指定需要执行的 sql 语句：

    mysql -u用户名 -p密码 -D数据库名 -e"sql 语句"
    mysql -u$user -p$pass -D $db -e "select host from user;"

第二种是通过管道的方式：

将每一步需要执行的语句保存到 tmp.sql 中，最后在使用

`mysql -uuname  -ppwd  < tmp.sql`
