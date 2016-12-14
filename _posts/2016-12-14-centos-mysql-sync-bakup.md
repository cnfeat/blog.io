---
layout: post
title: 在 centos 下实现 mysql 双机热备
date: 2016-12-14
categories: blog
tags: [centos,mysql]
description: 在 centos 下实现 mysql 双机热备
---

## 前提条件：

要想实现双机的热备，主从数据库服务器的版本的需求：

1. 要实现热备mysql的版本都要高于3.2；

2. 作为从数据库的数据库版本可以高于主服务器数据库的版本，但是不可以低于主服务器的数据库版本。

注：MySQL 主从服务器最好使用相同的软件版本，以避免不不可预期的故障。

## 实现原理：

MySQL使用3个线程来执行复制功能，其中1个在主服务器上，另两个在从服务器上。

当发出 `START SLAVE` 时，从服务器创建一个I/O线程，以连接主服务器并让主服务器发送二进制日志。

主服务器创建一个线程将二进制日志中的内容发送到从服务器。

从服务器I/O线程读取主服务器 Binlog Dump 线程发送的内容并将该数据拷贝到从服务器数据目录中的本地文件中，即中继日志。

第3个线程是SQL线程，从服务器使用此线程读取中继日志并执行日志中包含的更新。

`SHOW PROCESSLIST` 语句可以查询在主服务器上和从服务器上发生的关于复制的信息。


注：实际项目中两台服务器可以互为主备，当其中一台服务器出现故障时，另外一台服务器接管主服务器上的应用，此时便需要mysql的实时 **双机互备** 。

### 服务器环境：

主服务器（253）：

    操作系统：centos 6.7
    ip：192.168.0.253
    web环境：LNMP一键安装包搭建
    mysql：5.1.69

从服务器（251）：

    操作系统：centos 6.7
    ip：192.168.0.251
    web环境：LNMP一键安装包搭建
    mysql：5.1.69

## 设置主服务器：

### 创建同步账号：

数据库能相互访问受限要创建 **可以远程登陆的账号**，通过这个远程账号才能实现数据同步。

首先登陆主服务器（253）mysql,然后用以下命令创建远程登陆账号（从服务器251登录用）：
 
`GRANT all privileges ON *.* TO backup@'192.168.0.251' IDENTIFIED BY  'password';`
 
此命令是在主服务器上运行，（如做双机互备，要在从服务器上也执行一次对应的指令，IP地址要改成主服的）。

### 语法解释：

    grant 权限1,权限2,…权限n on 数据库名称.表名称 to 远程登陆用户名@远程登陆地址 identified by ‘远程登陆连接口令’;

    权限1,权限2,…权限n代表select,insert,update,delete,create,drop,index,alter,grant,references,reload,shutdown,process,file等14个权限。
    当权限1,权限2,…权限n被all privileges或者all代替，表示赋予用户全部权限。

    当数据库名称.表名称被*.*代替，表示赋予用户操作服务器上所有数据库所有表的权限。

    用户地址可以是localhost，也可以是ip地址、机器名字、域名。也可以用’%’表示从任何地址连接。

    ‘连接口令’ 不能为空，否则创建失败。

注：

在主服务器上建立一个为从服务器进行复制使用的用户。该账户必须授予 **REPLICATION SLAVE** 权限，由于仅仅是进行复制使用所以可以不需要再授予任何其它权限。

    mysql> GRANT REPLICATION SLAVE ON *.* TO 'replication'@'192.168.0.251' IDENTIFIED BY 'slavepasswd';
    mysql> FLUSH PRIVILEGES; 

生产环境主库用户的授权, 授权 增删改查 权限。

    > GRANT SELECT,INSERT,UPDATE,DELETE ON *.* to 'user'@'%' identified by '123456';

生产环境从库的授权, 仅授权 查 权限。

    > GRANT SELECT ON *.* to 'user'@'%' identified by '123456';

### 配置服务器的数据库参数：

对于独立安装的mysql来说, 配置文件在 /etc/my.cnf，用vi或者vim打开编辑：

`vi /etc/my.cnf`

配置内容如下：

    [mysqld]

    log-bin=mysql-bin     # 二进制文件的开头标示，必写项
    server-id= 1          # 主服务器写1，从服务器写2，必写项，主从服务器的一定不能相同
    binlog-do-db=test     # 主服务器用，需要同步的数据库，如果有多个自行复制多行
    #binlog-ignore-db = mysql   # 主服务器用，不需要同步的数据库，多个数据库自行复制

    master-host=192.168.0.251   # 允许同步服务器的ip，双机互备的情形下主服务器写从服务器ip，从服务器写主服务器ip
    master-user=backup          # 用于同步的账号，在上一步中已经创建的那个远程登陆账号
    master-password=******      # 用于同步账号的密码，在上一步创建远程登陆账号时候设置的密码
    master-port=3306            # 同步服务器的mysql的端口
    master-connect-retry=3      # 同步的时间间隔，单位是秒

    replicate-do-db=test        # 从服务器用，需要同步的数据库，如果有多个自行复制多行
    #replicate-ignore-db=test   # 从服务器用，不需要同步的数据库，如果有多个自行复制多行

    slave-skip-errors=all       # 忽略所有类型的错误
    sync_binlog=1               # 每向二进制日志文件写入n条sql或n个事务后，把二进制日志的数据刷新到新磁盘上，如果为0则不主动刷新而有操作系统决定，为了保险起见，这里建议写成1
    auto_increment_increment=1  # 控制列中的值的增量值
    auto_increment_offset=1     # 控制AUTO_INCREMENT 列值的起点

注：Mysql版本从5.1.7以后开始就不支持 “master-host” 类似的参数

以上配置为主服务器的配置，理解了上面的这几项，从服务器就容易了，只需要修改master-host和server-id即可。这些项有的是存在的，有的是新增加的，有的是需要修改的，请自行核对。

如果mysql版本是5.1.7之后的，主服务器配置：

    server-id = 1
    log-bin=mysql-bin
    binlog-do-db = test (改成需要备份的数据库名)
    binlog-ignore-db = mysql
    binlog-ignore-db = information_schema
    sync_binlog = 1      # 每向二进制日志文件写入n条sql或n个事务后，这把二进制日志问及那的数据刷新到新磁盘上，如果为0则不主动刷新而有操作系统决定，为了保险起见，这里建议写成1
    expire_logs_days = 10
    max_binlog_size = 128m
    binlog_format = mixed


### 执行同步：

重启mysql服务，确认配置文件没有问题：

`service mysqld restart`
 
然后停止同步、锁定表、查看读取的二进制文件和position的值，然后执行 CHANGE MASTER TO 命令，步骤如下：
 
    MySQL> stop slave;                   # 停止同步
    MySQL> flush tables with read lock;  # 锁定表为只读状态
    MySQL> SHOW MASTER STATUS;           # 查看主服务器的日志名称和position的值
 
当FLUSH TABLES WITH READ LOCK所置读锁定有效时（即mysql客户端程序不退出)，读取主服务器上当前的二进制日志名和偏移量值。从返回结果获取 二进制文件名 和 position 的值。记录该值，设置从服务器时需要使用这些值。它们表示复制坐标，从服务器应从该点开始从主服务器上进行新的更新。

### 保持mysql客户端程序不要退出。开启另一个终端对主服务器数据目录做备份。

备份的方式有两种：

#### （1）cp全部的数据

如果主数据库的服务可以停止，那么直接cp数据文件应该最快的生成快照的方法

#### （2）mysqldump备份数据方法

    [root@localhost ~]# /usr/bin/mysqldump -uroot -p123456 test -l -F >/tmp/test.sql
    [root@node1 ~]# cd /usr/local/mysql/data
    [root@node1 ~]# tar -zcvf /tmp/mysql-wapnews.tar.gz ./wapnews 

把备份完毕的数据库备份复制到从服务器上。

CHANGE MASTER TO 这条命令把数据保存到 master.info 文件中去，在数据库存放的路径下。
在主服务器上的mysql命令下执行如下语句：
 
    change master to master_host='192.168.0.251',master_user='backup',master_password='password',master_log_file='mysql-bin.000001', master_log_pos=106;

#### 格式说明：

    change master to master_host=’主服务器上写备服务器ip，从服务器写主服务器ip’,master_user=’同步用户名’,master_password=’同步密码’,master_log_file=’上面获取到的二进制文件名’, master_log_pos=获取到的position的值;

从服务器自行编写。

### 取得快照并记录日志名和偏移量后，回到前一终端重新启用写活动：

    MySQL> start slave;
    MySQL> unlock tables;
 
如果正确，在主、备服务器上分别写入数据就能看到同步。配置结束。


## 设置从服务器：

停止从服务器上的mysqld服务，并编辑从服务器的配置文件：/etc/my.cnf 的[ mysqld ] 部分：

    # service mysql stop

我的从服务器配置

    server-id = 2
    replicate-do-db = test
    replicate-ignore-db = information_schema
    replicate-ignore-db = mysql
    binlog_cache_size = 512M
    binlog_format = mixed

将主服务器数据库备份恢复到从服务器的数据目录中。确保对这些文件和目录的权限正确。服务器 MySQL运行的用户必须能够读写文件，如同在主服务器上一样。

    [root@node2 ~]# cd /usr/local/mysql/data
    [root@node2 ~]# tar -zxvf mysql-wapnews.tar.gz
    [root@node2 ~]# chown -R mysql:mysql /usr/local/mysql/data/wapnews 

启动从服务器：

    # service mysql start

先停止slave进程(如果有运行的话)

    stop slave;

在从服务器上执行下面的语句，用你的系统的实际值替换选项值：

    mysql> CHANGE MASTER TO
          -> MASTER_HOST='master_host_name',
          -> MASTER_USER='replication_user_name',
          -> MASTER_PASSWORD='replication_password',
          -> MASTER_LOG_FILE='recorded_log_file_name',
          -> MASTER_LOG_POS=recorded_log_position;

启动从服务器线程：

    mysql> START SLAVE； 

执行这些程序后，从服务器应连接主服务器，并补充自从快照以来发生的任何更新。

## 检查主从复制是否正常运行

在从服务器上运行 `show processlist \G;` 命令，检查是否启动两个复制进程。
在从服务器上运行 `show slave status \G;` 命令，检查复制进程是否正确。

出现： `Slave_IO_Running: Yes` 和 `Slave_SQL_Running: Yes` 表示复制正常，

如果有一个显示是NO，请检查以上的主从设置步骤是否正确。如果出现复制错误，从服务器的错误日志（HOSTNAME.err）中也会出现错误消息。

----------


## 容易出现的问题：

### 1.同步用户创建不成功

现象：

执行“ `SHOW SLAVE STATUS;` ” 命令，返回带有“ `error connecting to master ‘backup@192.168.0.251:3306′ – retry-time: 60  retries: 86400` ” 错误。

解决：提示说明连接到主/备服务器失败。

在网上流传有三种分析：网络问题、配置问题、同步用户写错了。

用 phpmyadmin 可以打开 mysql 数据库中的 user 表，如果成功创建同步账户则在 user 表中会 存在此用户，如果创建失败则不会存在。

### 2.日志重置

现象：经过多次的测试或者试验会出现多个日志，需要重新设置。

解决：在mysql下执行

    mysql> reset master;   #清除日志 
    mysql> purge binary logs to 'mysql-bin.000018'; # 删除mysql-bin.000018之前的日志

或者直接用ssh工具登陆到服务器直接删除 **master.info、 mysql-bin.000001、 mysql-bin.index、 relay-log.info** 这几个文件，重启mysql后会自己生成。

### 3.show Slave status 查看技巧：

现象： `show Slave status \G;` 显示的内容太多，无法查看全。

解决：

分页显示结果，或重定向到文件里慢慢看：

    $ mysql -uroot -p -e "show Slave status \G;" | less

### 4.日志文件不正确：

如果报如下错误：

              Slave_IO_State:
                 ...
            Slave_IO_Running: No
           Slave_SQL_Running: No

可能是指定的日志文件不正确, 解决方法：

从机

    stop slave;

主机

    Flush logs;
    show master status;

从机

    change master to master_host='192.168.0.251',master_user='backup',master_password='password',master_log_file='mysql-bin.000001', master_log_pos=106;

    start slave;

或可能是防火墙的3306端口没有打开！关闭系统防火墙后再试试：

`service iptables stop`


## 主库由于硬件故障如何将从库提升为主库(一主多从)

(mysql slave)

### (1) 确保从机没有再同步的SQL语句即出现Has read all relay log再关闭从库IO_Threat进程

    # mysql -uroot -p123456
    > stop slave IO_THREAD

### (2) 关闭从库slave服务然后将其提升为主库

    > stop slave
    > reset master

### (3) 更换从库IP为故障主库IP(配置方法略)

### (4) 删除新的主库master.info和relay-log.info,防止下次重启还会按照从库启动

    # cd /usr/local/mysql/log
    # rm -rf master.info relay-log.info

### (5) 重新配置从库连接主库的账号同步信息以及在下级从库重新设置偏移量保持与新的主库一致即可。

最后待主库硬件恢复，将其再设置为从库，并更换为上述从库IP地址, 完成主从切换。