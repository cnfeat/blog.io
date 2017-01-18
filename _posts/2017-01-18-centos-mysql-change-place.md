---
layout: post
title: CentOS Linux更改MySQL数据库目录位置具体操作
date: 2017-01-18
categories: blog
tags: [mysql]
description: CentOS Linux更改MySQL数据库目录位置具体操作
---

### 检查mysql数据库存放目录方法：

进入数据库

    mysql -u root -prootadmin

查看sql存储路径（查看datadir 那一行所指的路径）

    show variables like '%dir%';

### 把 MySQL 从 `/var/lib/mysql` 目录下面转移到 `/home/mysql_data/mysql` 目录的具体操作： 

#### 1、首先我们需要关闭MySQL，命令如下： 

    service mysqld stop 

#### 2、然后是转移数据，为了安全其见，我们采用复制命令cp，先找到mysql的原目录 

    cd /var/lib 
    ls 

运行这个命令之后就会看到mysql的目录了，然后执行cp命令 

    cp -a mysql /data/ 

这样就把数据库复制到 /data 下面去了 

注意： (**-a这个参数一定要带着，否则复制过去的权限就不对了。**)

#### 3、然后我们修改配置文件，一共有三个，下面我一一说明： 

修改第一个文件：修改之前先备份

    cp /etc/my.cnf /etc/my.cnf.bak 
    vi /etc/my.cnf 

打开之后修改 datadir 的目录为 `/data/mysql `

    datadir         = /data/mysql

把 socket 改成 `/data/mysql/mysql.sock`，为了安全起见，你可以把原来的注释掉，然后重新加入一行，改成现在的目录。 

    socket          = /data/mysql/mysql.sock

即：

    [client]
    #socket         = /var/lib/mysql/mysql.sock
    socket          = /data/mysql/mysql.sock

[mysqld] 模块下

    datadir         = /data/mysql
    socket          = /data/mysql/mysql.sock


修改第二个文件：修改之前先备份 

    cp /etc/init.d/mysqld /etc/init.d/mysqld.bak 
    vi /etc/init.d/mysqld 

注意：准确的位置是 `/etc/rc.d/init.d/mysqld`，由于这里这里有一个 `/etc/init.d` 到 `/etc/rc.d/init.d` 的映射，所以用上面的命令即可，也简单。
 
把其中 `datadir=/var/lib/mysql` 一行中，等号右边的路径改成你现在的实际存放路径：`/data/mysql`

即把 `get_mysql_option mysqld datadir "/var/lib/mysql" `改成

`get_mysql_option mysqld datadir "/data/mysql"`

修改第三个文件：修改之前先备份 

    cp /usr/bin/mysqld_safe /usr/bin/mysqld_safe.bak 
    vi /usr/bin/mysqld_safe 

这里也是修改 datadir 的目录为 `/data/mysql`

即把 `DATADIR=/var/lib/mysql` 改为 `DATADIR=/data/mysql`


#### 4、下面需要建立一个mysql.sock的链接： 

    ln -s /data/mysql/mysql.sock /var/lib/mysql/mysql.sock 

至此所有的修改都完成了，下面启动mysql 

    service mysqld start 

或者重启linux 

    reboot
 
如果能正常启动，说明修改成功。