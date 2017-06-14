---
layout: post
title: keepalived实现双机热备(主备自动切换)
date: 2017-06-13
categories: blog
tags: [operations]
description: keepalived实现双机热备(主备自动切换)
---

### 什么是双机热备？

通常说的双机热备是指两台机器都在运行，但并不是两台机器都同时在提供服务。

当提供服务的一台出现故障的时候，另外一台会马上自动接管并且提供服务，而且切换的时间非常短。

### 双机热备原理：

下面来以 `keepalived` 结合 `tomcat` 来实现一个 `web 服务器` 的双机热备。

keepalived 的工作原理是 `VRRP（Virtual Router Redundancy Protocol）虚拟路由冗余协议`。

在 VRRP 中有两组重要的概念：`VRRP路由器`和`虚拟路由器`，`主控路由器`和`备份路由器`。

**VRRP路由器**是指运行VRRP的路由器，是物理实体。

**虚拟路由器**是指VRRP协议创建的，是逻辑概念。一组VRRP路由器协同工作，共同构成一台虚拟路由器。

Vrrp中存在着一种选举机制，用以选出提供服务的路由即**主控路由**，其他的则成了**备份路由**。当主控路由失效后，备份路由中会重新选举出一个主控路由，来继续工作，来保障不间断服务。

### 我们在本文中的测试环境如下：

两台物理服务器和一个虚拟服务器(vip):

	master：redhat 2.6.18-53.el5  192.168.8.4
	backup: redhat 2.6.18-53.el5  192.168.8.6
	vip： 192.168.8.100

测试环境的网络topology图如下:

![](https://azraelgreen.github.io/img/20170613_keepalived.gif)

	节点A 192.168.8.4 (主节点)
	节点B 192.168.8.6(备用节点)
	虚拟IP(对外提供服务的IP 192.168.8.100)

在这种模式下，虚拟IP在某时刻只能属于某一个节点，另一个节点作为备用节点存在。

当主节点不可用时，备用节点接管虚拟IP（即虚拟IP漂移至节点B），提供正常服务。

### keepalived的原理可以这样简单理解:

keepalived安装在两台物理服务器上，并相互监控对方是否在正常运行。

#### 当节点A正常的时候:

节点A上的keepalived会将下面的信息广播出去:

	192.168.8.100 这个IP对应的MAC地址为 节点A网卡的MAC地址

图中的其它电脑如客户端和NodeB会更新自己的ARP表，

对应 `192.168.8.100的MAC地址 = 节点A网卡的MAC地址`。

#### 当节点A发生故障的时候，

节点B上的keepalived会检测到，并且将下面的信息广播出去:

	192.168.8.100 这个IP对应的MAC地址 为节点B网卡的MAC地址

图中的其它电脑如客户端会更新自己的ARP表，

对应 `192.168.8.100的MAC地址 = 节点B网卡的MAC地址`。

### keepalived 配置方法：

#### 1、在主备机器上安装 keepalived，步骤如下:

下载keepalived-1.1.15.tar.gz，然后解压安装

	tar zxvf keepalived-1.1.15.tar.gz
	cd keepalived-1.1.15
	./configure
	make
	make install

#### keepalived 的配置文件介绍

keepalived只有一个配置文件 `keepalived.conf`，里面主要包括以下几个配置区域：

分别是global_defs、static_ipaddress、static_routes、vrrp_script、vrrp_instance和virtual_server。


#### 2、配置keepalived

配置中的 `state MASTER` 决定了节点为主节点

`priority` 决定了优先级，比如在有多个备用节点的时候，主节点故障后优先级值大的接管。

**主节点的配置**如下:

	global_defs {  
		router_id NodeA  
	}  
	vrrp_instance VI_1 {  
		state MASTER    #设置为主服务器  
		interface eth0  #监测网络接口  
		virtual_router_id 51  #主、备必须一样  
		priority 100   #(主、备机取不同的优先级，主机值较大，备份机值较小,值越大优先级越高)  
		advert_int 1   #VRRP Multicast广播周期秒数  
		authentication {  
		auth_type PASS  #VRRP认证方式，主备必须一致  
		auth_pass 1111   #(密码)  
	}  
	virtual_ipaddress {  
		192.168.8.100/24  #VRRP HA虚拟地址  
	}  

**备用节点的配置**如下:


	global_defs {  
		router_id NodeB  
	}  
	vrrp_instance VI_1 {  
		state BACKUP    #设置为辅服务器  
		interface eth0  #监测网络接口  
		virtual_router_id 51  #主、备必须一样  
		priority 90   #(主、备机取不同的优先级，主机值较大，备份机值较小,值越大优先级越高)  
		advert_int 1   #VRRP Multicast广播周期秒数  
		authentication {  
		auth_type PASS  #VRRP认证方式，主备必须一致  
		auth_pass 1111   #(密码)  
	}  
	virtual_ipaddress {  
		192.168.8.100/24  #VRRP HA虚拟地址  
	}  

#### 3、启动keepalived:

`keepalived -D -f /usr/local/etc/keepalived/keepalived.conf`

查看log消息:

`tail -f /var/log/messages`

##### 启动主节点A后的日志为: 会广播ARP消息

	[root@srv4 ~]# tail -f /var/log/messages  
	Sep 20 01:45:29 srv4 Keepalived_vrrp: Configuration is using : 34546 Bytes  
	Sep 20 01:45:29 srv4 Keepalived_vrrp: VRRP sockpool: [ifindex(2), proto(112), fd(8,9)]  
	Sep 20 01:45:30 srv4 Keepalived_vrrp: VRRP_Instance(VI_1) Transition to MASTER STATE  
	Sep 20 01:45:31 srv4 Keepalived_vrrp: VRRP_Instance(VI_1) Entering MASTER STATE  
	Sep 20 01:45:31 srv4 Keepalived_vrrp: VRRP_Instance(VI_1) setting protocol VIPs.  
	Sep 20 01:45:31 srv4 Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth0 for 192.168.8.100  
	Sep 20 01:45:31 srv4 Keepalived_vrrp: Netlink reflector reports IP 192.168.8.100 added  
	Sep 20 01:45:31 srv4 Keepalived_healthcheckers: Netlink reflector reports IP 192.168.8.100 added  
	Sep 20 01:45:31 srv4 avahi-daemon[4029]: Registering new address record for 192.168.8.100 on eth0.  
	Sep 20 01:45:36 srv4 Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth0 for 192.168.8.100  

通过 `ip a` 命令可以看到 `192.168.8.100/24` 绑定到了 `eth0` 上

	[root@srv4 bin]# ip a  
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue   
		link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
		inet 127.0.0.1/8 scope host lo  
		inet6 ::1/128 scope host   
			valid_lft forever preferred_lft forever  
	2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000  
		link/ether 00:0c:29:50:2d:9d brd ff:ff:ff:ff:ff:ff  
		inet 192.168.8.4/24 brd 192.168.8.255 scope global eth0  
		inet 192.168.8.100/24 scope global secondary eth0  
		inet6 fe80::20c:29ff:fe50:2d9d/64 scope link   
			valid_lft forever preferred_lft forever  

##### 启动备用节点B后的日志为:

	Sep 20 01:47:31 hadoopsrv Keepalived_vrrp: Configuration is using : 34262 Bytes  
	Sep 20 01:47:31 hadoopsrv Keepalived_vrrp: VRRP_Instance(VI_1) Entering BACKUP STATE  
	Sep 20 01:47:31 hadoopsrv Keepalived_vrrp: VRRP sockpool: [ifindex(2), proto(112), fd(7,8)]  
	Sep 20 01:47:31 hadoopsrv Keepalived: Starting VRRP child process, pid=20567  


#### 4、在两台机器上安装tomcat，安装步骤省略

安装完成后在节点A的机器上创建一个html文件内容如下

	this is the test page  
	<br>  
	from server 192.168.8.4  

通过下面的url验证能够正常访问

	http://192.168.8.4:8080/test/test.html

安装完成后在节点B的机器上创建一个html文件内容如下

	this is the test page  
	<br>  
	from server 192.168.8.6  

通过下面的url验证能够正常访问

http://192.168.8.6:8080/test/test.html

##### 在主节点，节点A正常的时候通过下面的url访问

	192.168.8.100:8080/test/test.html

返回的内容应该为主节点上的html

	this is the test page  
	<br>  
	from server 192.168.8.4  

##### 将节点A的keepalived停止: 

`killall keepalived`

通过下面的url访问

	192.168.8.100:8080/test/test.html

返回的内容应该为备用节点上的内容

	this is the test page  
	<br>  
	from server 192.168.8.6  

##### 同时查看节点B的日志:

发现节点B转为主节点并且会广播ARP消息

	Sep 20 01:55:44 hadoopsrv Keepalived_vrrp: VRRP_Instance(VI_1) Transition to MASTER STATE  
	Sep 20 01:55:45 hadoopsrv Keepalived_vrrp: VRRP_Instance(VI_1) Entering MASTER STATE  
	Sep 20 01:55:45 hadoopsrv Keepalived_vrrp: VRRP_Instance(VI_1) setting protocol VIPs.  
	Sep 20 01:55:45 hadoopsrv Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth0 for 192.168.8.100  
	Sep 20 01:55:45 hadoopsrv avahi-daemon[3769]: Registering new address record for 192.168.8.100 on eth0.  
	Sep 20 01:55:50 hadoopsrv Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth0 for 192.168.8.100  


本文的目的，主要是演示keepalived实现双机热备的功能和过程。
