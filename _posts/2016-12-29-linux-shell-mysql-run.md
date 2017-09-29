---
layout: post
title: Shell 脚本中执行 mysql 语句
date: 2016-12-29
categories: blog
tags: [mysql]
description: Shell 脚本中执行 mysql 语句
---

对于自动化运维，诸如备份恢复之类的，DBA经常需要将SQL语句封装到shell脚本。

## 本文描述了在Linux环境下mysql数据库中，shell脚本下调用sql语句的几种方法：
 
### 1、将SQL语句直接嵌入到shell脚本文件中

`[root@SZDB ~]# vi shell_call_sql1.sh`

    #!/bin/bash  
	# Define log  
	TIMESTAMP=`date +%Y%m%d%H%M%S`  
	LOG=call_sql_${TIMESTAMP}.log  
	echo "Start execute sql statement at `date`." >>${LOG}  
	  
	# execute sql stat  
	mysql -uroot -p123456 -e "  
	tee /tmp/temp.log  
	drop database if exists tempdb;  
	create database tempdb;  
	use tempdb  
	create table if not exists tb_tmp(id smallint,val varchar(20));  
	insert into tb_tmp values (1,'jack'),(2,'robin'),(3,'mark');  
	select * from tb_tmp;  
	notee  
	quit"  
	  
	echo -e "\n">>${LOG}  
	echo "below is output result.">>${LOG}  
	cat /tmp/temp.log>>${LOG}  
	echo "script executed successful.">>${LOG}  
	exit;  

`[root@SZDB ~]# ./shell_call_sql1.sh`

### 2、命令行调用单独的SQL文件

`[root@SZDB ~]# vi temp.sql`

	tee /tmp/temp.log  
	drop database if exists tempdb;  
	create database tempdb;  
	use tempdb  
	create table if not exists tb_tmp(id smallint,val varchar(20));  
	insert into tb_tmp values (1,'jack'),(2,'robin'),(3,'mark');  
	select * from tb_tmp;  
	notee  

`[root@SZDB ~]# mysql -uroot -p123456 -e "source /root/temp.sql"`

### 3、使用管道符调用SQL文件

`[root@SZDB ~]# mysql -uroot -p123456 </root/temp.sql`
	  
使用管道符调用SQL文件以及输出日志

`[root@SZDB ~]# mysql -uroot -p123456 </root/temp.sql >/tmp/temp.log`

### 4、shell脚本中MySQL提示符下调用SQL

`[root@SZDB ~]# vi shell_call_sql2.sh`

	#!/bin/bash  
	mysql -uroot -p123456 <<EOF  
	source /root/temp.sql;  
	select current_date();  
	delete from tempdb.tb_tmp where id=3;  
	select * from tempdb.tb_tmp where id=2;  
	EOF  
	exit;  

`[root@SZDB ~]# ./shell_call_sql2.sh`