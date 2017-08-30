---
layout: post
title: 如何在 CentOS 7 上将 MySQL 数据目录更改为新位置
date: 2017-08-30
categories: blog
tags: [mysql,centos]
description: 如何在 CentOS 7 上将 MySQL 数据目录更改为新位置
---

> 数据库随时间增长，有时会超过文件系统上的空间。当它们位于与操作系统的其余部分相同的分区上时，也可能遇到I/O争用。所以需要重新定位MySQL的数据目录。

在这个例子中，我们将数据移动到安装在块存储设备/mnt/volume-nyc1-01 。 

### 第1步 – 移动MySQL数据目录

先让我们通过使用管理凭据启动交互式 MySQL 会话来验证当前位置：

	mysql -u root -p

出现提示时，提供MySQL root密码。

然后从MySQL提示符中选择数据目录：

	select @@datadir;

输出：

	+-----------------+
	| @@datadir       |
	+-----------------+
	| /var/lib/mysql/ |
	+-----------------+
	1 row in set (0.00 sec)

此输出证实的MySQL被配置为使用默认的数据目录， /var/lib/mysql/,所以这是我们需要移动的目录。

为了确保数据的完整性，我们将在我们实际修改数据目录之前关闭MySQL：

	sudo systemctl stop mysqld

systemctl不显示所有服务管理命令的结果，所以如果你想确保你已经成功了，请使用以下命令：

	sudo systemctl status mysqld

如果输出的最后一行告诉您服务器已停止，您可以确定它已关闭：

	Output. . .Jul 18 11:24:20 ubuntu-512mb-nyc1-01 systemd[1]: Stopped MySQL Community Server.

现在，服务器关闭，我们将现有的数据库目录复制到新的位置。

#### 方法一：用 `rsync`

使用 `-a` 标志保留的权限和其他目录属性，而 `-v` 提供详细输出，以便能够按照进度。

**注意：确保没有对目录没有尾随斜线。**当有一个结尾的斜线， rsync将转储目录复制到安装点，而不是转移成一个包含内容mysql目录。

	sudo rsync -av /var/lib/mysql /mnt/volume-nyc1-01

#### 方法二：直接使用 cp 命令

	cp -a /var/lib/mysql  /mnt/volume-nyc1-01


下面需要建立一个mysql.sock的链接：
ln -s /home/mysql/mysql.sock /var/lib/mysql/mysql.sock


### 第2步 – 指向新数据位置

编辑配置文件，修改为新的数据目录：

	sudo vi /etc/my.cnf

修改内容：

	[mysqld]
	. . .
	datadir=/mnt/volume-nyc1-01/mysql
	socket=/mnt/volume-nyc1-01/mysql/mysql.sock
	. . .

增加配置的mysql客户端。在文件的底部插入下面的设置：

	[client]port=3306
	socket=/mnt/volume-nyc1-01/mysql/mysql.sock

然后键入 `:wq!` 保存并退出文件。

### 第3步 – 重新启动MySQL

启动MySQL并验证是否正常使用：

	sudo systemctl start mysqld
	sudo systemctl status mysqld

要确保新的数据目录确实在使用，请启动MySQL监视器。

	mysql -u root -p

再次查看数据目录的值：

	select @@datadir;

输出：

	+----------------------------+
	| @@datadir                  |
	+----------------------------+
	| /mnt/volume-nyc1-01/mysql/ |
	+----------------------------+
	1 row in set (0.01 sec)

现在您已重新启动MySQL并确认它正在使用新位置。
