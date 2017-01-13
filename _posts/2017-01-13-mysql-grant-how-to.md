---
layout: post
title: MySQL grant 语法的详细解析
date: 2017-01-13
categories: blog
tags: [mysql]
description: MySQL grant 语法的详细解析
---

我们大家都知道MySQL数据库赋予用户权限命令的简单格式可概括为：

**grant 权限 on 数据库对象 to 用户**

### 一、grant 普通数据用户，查询、插入、更新、删除 数据库中所有表数据的权利。

    grant select on testdb.* to common_user@'%'  
    grant insert on testdb.* to common_user@'%'  
    grant update on testdb.* to common_user@'%'  
    grant delete on testdb.* to common_user@'%' 

或者，用一条 MySQL 命令来替代：

    grant select, insert, update, delete on testdb.* to common_user@'%' 

### 二、grant 数据库开发人员，创建表、索引、视图、存储过程、函数。。。等权限。

grant 创建、修改、删除 MySQL 数据表结构权限。

    grant create on testdb.* to developer@'192.168.0.%';  
    grant alter on testdb.* to developer@'192.168.0.%';  
    grant drop on testdb.* to developer@'192.168.0.%'; 

MySQLgrant 操作 MySQL 外键权限。

    grant references on testdb.* to developer@'192.168.0.%';

grant 操作 MySQL 临时表权限。

    grant create temporary tables on testdb.* to

grant 操作 MySQL 索引权限。

    grant index on testdb.* to

grant 操作 MySQL 视图、查看视图源代码 权限。

    grant create view on testdb.* to developer@'192.168.0.%';  
    grant show view on testdb.* to developer@'192.168.0.%'; 

grant 操作 MySQL 存储过程、函数 权限。

    grant create routine on testdb.* to developer@'192.168.0.%'; 
    grant alter routine on testdb.* to developer@'192.168.0.%'; 
    grant execute on testdb.* to developer@'192.168.0.%'; 

### 三、grant 普通 DBA 管理某个 MySQL 数据库的权限。

    MySQLgrant all privileges on testdb to

其中，关键字 “privileges” 可以省略。

    grant execute on procedure testdb.pr_add to 'dba'@'localhost'  
    grant execute on function testdb.fn_add to 'dba'@'localhost'  
    grant all on *.* to dba@'localhost' 

### 四、MySQL grant 权限，分别可以作用在多个层次上。

#### 1.grant 作用在整个 MySQL 服务器上：

    grant select on *.* to dba@localhost; -- dba 可以查询 MySQL 中所有数据库中的表。

    grant all on *.* to dba@localhost; -- dba 可以管理 MySQL 中的所有数据库

#### 2.grant 作用在单个数据库上：

    grant select on testdb.* to dba@localhost; -- dba 可以查询 testdb 中的表。

#### 3.grant 作用在单个数据表上：

    grant select, insert, update, delete on testdb.orders to

#### 4.grant 作用在表中的列上：

    grant select(id, se, rank) on testdb.apache_log to

#### 5.MySQL grant 作用在存储过程、函数上：

    grant execute on procedure testdb.pr_add to 'dba'@'localhost'  
    grant execute on function testdb.fn_add to 'dba'@'localhost'
 
### 五、查看 MySQL 用户权限

查看当前用户（自己）权限：

    show grants;

查看其他 MySQL 用户权限：

    show grants for dba@localhost;

### 六、撤销已经赋予给 MySQL 用户权限的权限。

revoke 跟 grant 的语法差不多，只需要把关键字 “to” 换成 “from” 即可：

    grant all on *.* to dba@localhost;  
    revoke all on *.* from dba@localhost; 

### 七、MySQL grant、revoke 用户权限注意事项

#### 1.grant, revoke 用户权限后，该用户只有重新连接 MySQL 数据库，权限才能生效。

#### 2.如果想让授权的用户，也可以将这些权限 MySQLgrant 给其他用户，需要选项 “grant option“

    grant select on testdb.* to dba@localhost with grant option;

这个特性一般用不到。实际中，数据库权限最好由 DBA 来统一管理。
 