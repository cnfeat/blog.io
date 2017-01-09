---
layout: post
title: mysql查看数据库和表的占用空间大小
date: 2017-01-09
categories: blog
tags: [mysql]
description: mysql查看数据库和表的占用空间大小
---

### 1.查看数据库的大小

    use 数据库名
    SELECT sum(DATA_LENGTH)+sum(INDEX_LENGTH)
    FROM information_schema.TABLES where TABLE_SCHEMA='数据库名';

得到的**结果是以字节为单位**，除1024为K，除1048576为M。

### 2.查看表的最后mysql修改时间

    select TABLE_NAME,UPDATE_TIME from INFORMATION_SCHEMA.tables where TABLE_SCHEMA='数据库名';

可以通过查看数据库中表的mysql修改时间，来确定mysql数据库是否已经长期不再使用。

### 3.查看数据库中各个表占用的空间大小
 
如果想知道MySQL数据库中每个表占用的空间、表记录的行数的话，可以打开MySQL的 information_schema 数据库。在该库中有一个 TABLES 表，这个表主要字段分别是：

    TABLE_SCHEMA : 数据库名
    TABLE_NAME：表名
    ENGINE：所使用的存储引擎
    TABLES_ROWS：记录数
    DATA_LENGTH：数据大小
    INDEX_LENGTH：索引大小

其他字段请参考MySQL的手册，我们只需要了解这几个就足够了。

所以要知道一个表占用空间的大小，那就相当于是 数据大小 + 索引大小 即可。

SQL:

    SELECT TABLE_NAME,DATA_LENGTH+INDEX_LENGTH,TABLE_ROWS FROM TABLES WHERE TABLE_SCHEMA='数据库名' AND TABLE_NAME='表名'

 