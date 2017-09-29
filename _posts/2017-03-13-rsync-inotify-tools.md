---
layout: post
title: Linux下Rsync+Inotify-tools实现数据实时同步
date: 2017-03-13
categories: blog
tags: [rsync]
description: Linux下Rsync+Inotify-tools实现数据实时同步
---

## 操作环境说明：

操作系统：CentOS 6.X

源服务器：192.168.0.111

目标服务器：192.168.0.222

目的：把源服务器上/home/www目录实时同步到目标服务器的/home/www下

## 第一部分：在目标服务器192.168.0.222上操作

### 一、在目标服务器安装Rsync服务端

#### 1、关闭SELINUX

	vi /etc/selinux/config #编辑防火墙配置文件

	#SELINUX=enforcing #注释掉

	#SELINUXTYPE=targeted #注释掉

	SELINUX=disabled #增加

	:wq! #保存，退出

	setenforce 0  #立即生效

#### 2、开启防火墙tcp 873端口（Rsync默认端口）

	vi /etc/sysconfig/iptables #编辑防火墙配置文件

	-A INPUT -m state --state NEW -m tcp -p tcp --dport 873 -j ACCEPT

	:wq! #保存，退出

	/etc/init.d/iptables restart #最后重启防火墙使配置生效

#### 3、安装Rsync服务端软件

	yum install rsync xinetd #安装

	vi /etc/xinetd.d/rsync #编辑配置文件，设置开机启动rsync

	disable = no #修改为no

	:wq! #保存退出

	/etc/init.d/xinetd start #启动（CentOS中是以xinetd来管理Rsync服务的）

#### 4、创建rsyncd.conf配置文件

	vi /etc/rsyncd.conf #创建配置文件，添加以下代码

	log file = /var/log/rsyncd.log #日志文件位置，启动rsync后自动产生这个文件，无需提前创建
	pidfile = /var/run/rsyncd.pid  #pid文件的存放位置
	lock file = /var/run/rsync.lock  #支持max connections参数的锁文件
	secrets file = /etc/rsync.pass  #用户认证配置文件，里面保存用户名称和密码，后面会创建这个文件
	motd file = /etc/rsyncd.Motd  #rsync启动时欢迎信息页面文件位置（文件内容自定义）

	[www] #自定义名称
	path = /home/www/ #rsync服务端数据目录路径
	comment = home_www.xxx.com #模块名称与[home_www.xxx.com]自定义名称相同
	uid = root #设置rsync运行权限为root
	gid = root #设置rsync运行权限为root
	port=873  #默认端口
	use chroot = no #默认为true，修改为no，增加对目录文件软连接的备份
	read only = no  #设置rsync服务端文件为读写权限
	list = no #不显示rsync服务端资源列表
	max connections = 200 #最大连接数
	timeout = 600  #设置超时时间
	auth users = admin #执行数据同步的用户名，可以设置多个，用英文状态下逗号隔开
	hosts allow = 192.168.0.111  #允许进行数据同步的客户端IP地址，可以设置多个，用英文状态下逗号隔开
	（hosts allow = 192.168.16.0/255.255.255.0） #允许一个网段的IP地址
	hosts deny = 192.168.X.X #禁止数据同步的客户端IP地址，可以设置多个，用英文状态下逗号隔开

	:wq!  #保存,退出

#### 5、创建用户认证文件

	vi /etc/rsync.pass #配置文件，添加以下内容

	admin:your_password #格式，用户名:密码，可以设置多个，每行一个用户名:密码

	:wq!  #保存，退出

#### 6、设置文件权限

	chmod 600 /etc/rsyncd.conf  #设置文件所有者读取、写入权限
	chmod 600 /etc/rsync.pass  #设置文件所有者读取、写入权限

#### 7、启动rsync

	/etc/init.d/xinetd start  #启动
	service xinetd stop   #停止
	service xinetd restart #重新启动

## 第二部分：在源服务器192.168.0.111上操作

### 一、安装Rsync客户端

#### 1、关闭SELINUX

	vi /etc/selinux/config #编辑防火墙配置文件

	#SELINUX=enforcing #注释掉

	#SELINUXTYPE=targeted #注释掉

	SELINUX=disabled #增加

	:wq! #保存，退出

	setenforce 0 #立即生效

#### 2、开启防火墙tcp 873端口（Rsync默认端口，做为客户端的Rsync可以不用开启873端口）

	vi /etc/sysconfig/iptables #编辑防火墙配置文件

	-A INPUT -m state --state NEW -m tcp -p tcp --dport 873 -j ACCEPT

	:wq! #保存，退出

	/etc/init.d/iptables restart #最后重启防火墙使配置生效

#### 3、安装Rsync客户端软件

	whereis rsync   #查看系统是否已安装rsync,出现下面的提示，说明已经安装
	rsync: /usr/bin/rsync /usr/share/man/man1/rsync.1.gz
	yum install  xinetd  #只安装xinetd即可，CentOS中是以xinetd来管理rsync服务的
	yum install rsync xinetd #如果默认没有rsync，运行此命令进行安装rsync和xinetd

	vi /etc/xinetd.d/rsync #编辑配置文件，设置开机启动rsync

	disable = no #修改为no

	/etc/init.d/xinetd start #启动（CentOS中是以xinetd来管理rsync服务的）

#### 4、创建认证密码文件

	vi /etc/passwd.txt  #编辑文件，添加以下内容

	your_password #密码

	:wq! #保存退出

	chmod 600 /etc/passwd.txt #设置文件权限，只设置文件所有者具有读取、写入权限即可

#### 5、测试源服务器192.168.0.111到两台目标服务器192.168.0.222之间的数据同步

	mkdir /home/www/test #在源服务器上创建测试文件夹，然后在源服务器运行下面命令

	rsync -avH --port=873 --progress --delete  /home/www/  admin@192.168.0.222::www --password-file=/etc/passwd.txt

运行完成后，在目标服务器192.168.0.222上查看，在/home/www目录下有test文件夹，说明数据同步成功。

### 二、安装Inotify-tools工具，实时触发rsync进行同步

#### 1、查看服务器内核是否支持inotify

	ll /proc/sys/fs/inotify   #列出文件目录，出现下面的内容，说明服务器内核支持inotify

	-rw-r--r-- 1 root root 0 Mar  7 02:17 max_queued_events
	-rw-r--r-- 1 root root 0 Mar  7 02:17 max_user_instances
	-rw-r--r-- 1 root root 0 Mar  7 02:17 max_user_watches

备注：Linux下支持inotify的内核最小为2.6.13，可以输入命令：`uname -a` 查看内核

CentOS 6.X 内核默认已经支持inotify

#### 2、安装inotify-tools

	yum install make  gcc gcc-c++  #安装编译工具

inotify-tools下载地址：[http://github.com/downloads/rvoicilas/inotify-tools/inotify-tools-3.14.tar.gz](http://github.com/downloads/rvoicilas/inotify-tools/inotify-tools-3.14.tar.gz)

上传 inotify-tools-3.14.tar.gz 到 `/usr/local/src` 下(也可以是其他目录)

	cd /usr/local/src
	tar zxvf inotify-tools-3.14.tar.gz  #解压
	cd inotify-tools-3.14 #进入解压目录

	./configure --prefix=/usr/local/inotify  #配置
	make  #编译
	make install  #安装

#### 3、设置系统环境变量，添加软连接

	echo "PATH=/usr/local/inotify/bin:$PATH" >>/etc/profile.d/inotify.sh
	source /etc/profile.d/inotify.sh  #使设置立即生效
	echo "/usr/local/inotify/lib" >/etc/ld.so.conf.d/inotify.conf
	ln -s /usr/local/inotify/include  /usr/include/inotify

#### 4、修改inotify默认参数（inotify默认内核参数值太小）

##### 查看系统默认参数值

	sysctl -a | grep max_queued_events

结果是：`fs.inotify.max_queued_events = 16384`

	sysctl -a | grep max_user_watches

结果是：`fs.inotify.max_user_watches = 8192`

	sysctl -a | grep max_user_instances

结果是：`fs.inotify.max_user_instances = 128`

##### 修改参数：

	sysctl -w fs.inotify.max_queued_events="99999999"
	sysctl -w fs.inotify.max_user_watches="99999999"
	sysctl -w fs.inotify.max_user_instances="65535"

	vi /etc/sysctl.conf #添加以下代码

	fs.inotify.max_queued_events=99999999
	fs.inotify.max_user_watches=99999999
	fs.inotify.max_user_instances=65535

	:wq! #保存退出

##### 参数说明：

`max_queued_events`：

inotify队列最大长度，如果值太小，会出现"** Event Queue Overflow **"错误，导致监控文件不准确

`max_user_watches`：

要同步的文件包含多少目录，可以用：`find /home/www -type d | wc -l` 统计，必须保证 `max_user_watches` 值大于统计结果（这里 `/home/www` 为同步文件目录）

`max_user_instances`：

每个用户创建inotify实例最大值

#### 5、创建脚本，实时触发rsync进行同步

	vi /usr/local/inotify/rsync.sh   #编辑，添加以下代码

	#!/bin/sh
 
	srcdir=/home/www/
	dstdir=www
	excludedir=/usr/local/inotify/exclude.list
	rsyncuser=admin
	rsyncpassdir=/etc/passwd.txt
	dstip="192.168.0.222"
	rsync -avH --port=873 --progress --delete  --exclude-from=$excludedir  $srcdir $rsyncuser@$dstip::$dstdir --password-file=$rsyncpassdir
	/usr/local/inotify/bin/inotifywait -mrq --timefmt '%d/%m/%y %H:%M' --format '%T %w%f%e' -e close_write,modify,delete,create,attrib,move $srcdir |  while read file
	do
    	rsync -avH --port=873 --progress --delete  --exclude-from=$excludedir  $srcdir $rsyncuser@$dstip::$dstdir --password-file=$rsyncpassdir
    echo "  ${file} was rsynced" >> /tmp/rsync.log 2>&1
	done

	chmod +x /usr/local/inotify/rsync.sh   #添加脚本执行权限
	touch /usr/local/inotify/exclude.list    #新建排除目录文件

##### 脚本参数说明：

	srcdir=/home/www/  #源服务器同步目录
	dstdir=www    #目标服务器rsync同步目录模块名称
	excludedir=/usr/local/inotify/exclude.list   

不需要同步的目录，如果有多个，每一行写一个目录，使用相对于同步模块的路径；

例如：不需要同步/home/www/目录下的a目录和b目录下面的b1目录，exclude.list文件可以这样写

a/

b/b1/

	rsyncuser=admin  #目标服务器rsync同步用户名
	rsyncpassdir=/etc/passwd.txt  #目标服务器rsync同步用户的密码在源服务器的存放路径
	dstip="192.168.16.240"  #目标服务器ip，多个ip用空格分开
	/tmp/rsync.log  #脚本运行日志记录

#### 6、设置脚本开机自动执行

	vi /etc/rc.d/rc.local  #编辑，在最后添加一行

	sh /usr/local/inotify/rsync.sh & ＃设置开机自动在后台运行脚本 （可用at命令定时运行）

	:wq!  #保存退出

#### 7、测试inotify实时触发rsync同步脚本是否正常运行

在源服务器192.168.0.111上创建文件夹test

	mkdir /home/www/ttt

重新启动源服务器：192.168.0.111

等系统启动之后，查看目标服务器192.168.0.222的/home/www下是否有ttt文件夹

然后再在源服务器192.168.0.111创建文件夹ttt_new

	mkdir /home/www/ttt_new

继续查看两台目标服务器192.168.21.127，192.168.21.128的/home/www下是否有ttt_new文件夹

如果以上测试都通过，说明inotify实时触发rsync同步脚本运行正常。

至此，Linux下Rsync+Inotify-tools实现数据实时同步完成。



#### inotify 参数

	-m 是保持一直监听
	-r 是递归查看目录
	-q 是打印出事件
	-e create,move,delete,modify,attrib 是指 “监听 创建 移动 删除 写入 权限” 事件

#### rsync参数

参见上一篇[《linux下rsync命令详解》](https://azraelgreen.github.io/blog/2017/03/10/rsync-commands/)