---
layout: post
title: Linux下杀病毒软件clamav使用配置
date: 2017-05-18
categories: blog
tags: [linux]
description: Linux下杀病毒软件clamav使用配置
---

clamav官方网站：[https://www.clamav.net/](https://www.clamav.net/)

### 一、YUM在线安装

	yum install clamav-server clamav-data clamav-update clamav-filesystem clamav clamav-scanner-systemd clamav-devel clamav-lib clamav-server-systemd -y
 

### 二、扫描被感染文件

	clamscan -r /etc/ >/home/etc.log
	#扫描配置目录并将日志保存到home下
	clamscan -r --bell -i  / >/home/all.log
	#全盘扫描显示有问题的结果，bell参数关闭屏显  

	grep -i found /home/all.log
	#查看结果,是否有被感染的文件

### 三、启用自动更新病毒库

	sed  -i  '$d'  /etc/sysconfig/freshclam
	#默认禁止自动更新，因此去除最后一行

	tail  -1 /etc/cron.d/clamav-update
	#查看自动更新计划任务
 

### 四、手动更新病毒库

	freshclam --verbose
	#升级病毒库

	cd /usr/local/clamav/update
	wget http://db.local.clamav.net/daily.cvd
	wget http://db.local.clamav.net/main.cvd
	wget http://db.local.clamav.net/bytecode.cvd
	#手动下载病毒库文件

	rm  /var/lib/clamav/mirrors.dat
	freshclam

	#“Update failed. Your network may be down or none of the mirrors listed in freshclam.conf is working”
	#删除掉旧的镜像地址文件，再手动更新一次病毒库
 

### 五、修改配置，开启服务

	sed -i -e  "s/^Example/#Example/"  /etc/freshclam.conf
	sed -i -e  "s/^Example/#Example/"  /etc/freshclam.conf 
	sed -i -e  "s/^Example/#Example/"  /etc/freshclam.conf
	sed -i -e  "s/^Example/#Example/"  /etc/freshclam.conf
	sed -i -e  "s/^Example/#Example/"  /etc/clamd.d/scan.conf

	echo  'LocalSocket  /var/run/clamd.scan/clamd.sock'  >>  /etc/clamd.d/scan.conf
	#替换和追加配置

	systemctl enable clamd@scan
	systemctl start clamd@scan
	#激活开机启动，启动服务

### 六、编译安装方式

	groupadd clamav
	useradd -g clamav -s /bin/false clamav
	#添加用户、禁止其登陆

	tar -xzvf clamav-0.80.tar.gz
	cd clamav-0.80
	./configure
	make check
	make install
	#编译安装测试

	echo > /tmp/clamscan.log
	#之前有日志 可以清空一下

	clamscan --recursive --log=/tmp/clamscan.log /
	#扫描全盘时间有点长，可以使用--bell -i 参数关闭屏显  

	grep  Infected /tmp/clamscan.log 
	#查看被感染文件数量
	grep -i found   /tmp/clamscan.log
	#查看被感染的文件
 