---
layout: post
title: mysql怎么开启INNODB数据引擎支持？
date: 2017-01-05
categories: blog
tags: [mysql]
description: mysql怎么开启INNODB数据引擎支持？
---

如果你在开发项目的时候用到了mysql数据库的innodb引擎，则需要调整一下mysql的默认配置。那么mysql怎么开启INNODB数据引擎支持呢？
 
### 首先，你安装的 mysql 需要支持 innodb 引擎才行

可以通过以下命令查看：

`mysql> show engines;`

1. 若结果中列 Engine 有 innodb 的记录，且列 Support 为 "YES" 则表示支持。
 
2. 若列 Engine 有 innodb 的记录，但且列 Support 为"NO",则说明你参数文件中屏蔽了 innodb 引擎的使用。你去参数配置文件中注释掉 "`#skip-innodb`" 项即可。（可以在配置文件 `/etc/mysql/my.cnf` 里面的进行配置）
 
3. 若列 Engine 没有 innodb 的记录，则表示你的版本没有支持 innodb 引擎，你必须去重新安装支持 innodb 引擎的版本。
 
### 开启InnoDB

关闭mysql的服务，在配置文件中添加以下内容

`#vi /etc/my.cnf`

    default-storage-engine=innodb
    default-table-type=innodb (在mysql 5.5.33中会导致无法启动，需注释掉)
    character-set-server=utf8
    collation-server=utf8_general_ci

保存后重启mysql服务。

PS：Mysql中默认的是MyISAM数据引擎，可惜此引擎不支持事务处理，我们需要将默认的数据引擎改为InnoDB。其中InnoDB和BerkeleyDB支持事务处理，只是默认的情况下都是被disable的。所有的引擎里面，InnoDB性能最强大，算是商业级的。