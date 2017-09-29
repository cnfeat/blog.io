---
layout: post
title: Linux操作系统单网卡双IP的设置
date: 2017-05-31
categories: blog
tags: [linux]
description: Linux操作系统单网卡双IP的设置
---

# 单网卡双IP单网关的配置方法：

## 方法一：

在 Centos 5 系统下：

### 1、配置第一个IP地址：

	[XXX]# cd /etc/sysconfig/network-scripts 
	[XXX]# vi ifcfg-eth0 

    DEVICE=eth0 
    BOOTPROTO=static 
    BROADCAST=192.168.80.255 	//*广播地址*// 
    IPADDR=192.168.80.189 		//*第一个IP地址*// 
    NETMASK=255.255.255.0 		//*网络掩码*// 
    NETWORK=192.168.80.0 		//*所在网段*// 
    ONBOOT=yes 

    :wq 		//*保存退出*//

### 2、复制第一个IP地址配置文件，为第二个IP地址配置文件，并修改里面的IP地址：

    [XXX]# cp ifcfg-eth0 ifcfg-eth1 
    [XXX]# vi ifcfg-eth1 

    DEVICE=eth0:1 	//用eth0也可以，也能通，但是重启网卡时会提示eth0 file existed！ 
    BOOTPROTO=static 
    BROADCAST=192.168.80.255 	//*广播地址*// 
    IPADDR=192.168.80.199 		//*第二个IP地址*// 
    NETMASK=255.255.255.0 		//*网络掩码*// 
    NETWORK=192.168.80.0 		//*所在网段*// 
    ONBOOT=yes 

    :wq //*保存退出*//

### 3、重启网卡： 

    [XXX]# service network restart 

或： 

    [XXX]# ifdown eth0 
    [XXX]# ifup eth0 

或 

    [XXX]# ifconfig eth0 down 
    [XXX]# ifconfig eth0 up

## 方法二：

在配置第二个IP地址的时候有变化

### 1、配置第一个IP地址以及网关：

	[XXX]# cd /etc/sysconfig/network-scripts 
    [XXX]# vi ifcfg-eth0 

    DEVICE=eth0 
    BOOTPROTO=static 
    BROADCAST=192.168.80.255 	//*广播地址*// 
    IPADDR=192.168.80.189 		//*第一个IP地址*// 
    NETMASK=255.255.255.0 		//*网络掩码*// 
    NETWORK=192.168.80.0 		//*所在网段*// 
    GATEWAY=192.168.80.1
	ONBOOT=yes 

    :wq 	//*保存退出*//

### 2：复制第一个IP地址配置文件为第二个IP地址配置文件，并修改里面的IP地址：

	[XXX]# cp ifcfg-eth0 ifcfg-eth0：1 		//变化的地方 
    [XXX]# vi ifcfg-eth0：1 		//变化的地方，这里与ubuntu系统不同没有ifcfg-eth0:0 

    DEVICE=eth0：1 			//变化的地方 
    BOOTPROTO=static 
    BROADCAST=192.168.80.255 	//*广播地址*// 
    IPADDR=192.168.80.199 		//*第二个IP地址*// 
    NETMASK=255.255.255.0 		//*网络掩码*// 
    NETWORK=192.168.80.0 		//*所在网段*// 
    ONBOOT=yes 

    :wq 	//*保存退出*//

### 注意：只能有一个网关！将网关地址配置在实际存在的 ifcfg-eth0 文件里，不要配置到虚拟出来的 ifcfg-eth0：1 之中。否则有问题。

在这里得到一个经验就是，配置网关可以直接在 ifcfg-eth0 或 ifcfg-eth1 这些配置文件里配置。与在 network 配置文件里配置没有什么不同。

## 这两种方法的优缺点：

第一种方法有个问题就是可能重启之后网卡 eth0 配置的能起来， eth1 可能起不来，同一网段的IP估计应该没问题，都是同一个网关。

要是双线，电信和联通双IP配置的话，只能**选用方法2比较保险一些**，方法1估计有点问题，没有测试过，不过记得有这么两种方法就是了。

只能配置电信或是联通的网关作为出口，这样就一定要确保eth0这个真实的网卡配置文件一定要是选作出口的电信或联通的IP配置，而那虚拟的ifcfg-eth1里边配置另一个。

# 单网卡双IP双网关的配置办法：

单网卡双IP的配置办法如上两种办法，双网关需要执行如下命令：

	route add –net 192.168.10.0 netmask 255.255.255.0 gw 192.168.10.1
	route add –net 192.168.20.0 netmask 255.255.255.0 gw 192.168.20.1

## CentOS 6 除了支持以上两种方法之外，新版本还支持如下这种配置方法：

在 centos6 中，有很多地方和之前的版本有所不同：

如果要在一个网卡上实现多IP地址，就可以简单的在相应的网卡配置文件中简单的添加如下设置，这里关键，必须的有。

	[root@localhost network-scripts]# vi ifcfg-eth1

	DEVICE="eth1"
	NM_CONTROLLED="yes"
	BOOTPROTO=none
	ONBOOT="yes"
	IPADDR="172.16.1.1"
	NETMASK="255.255.0.0"
	IPADDR2="172.16.1.2" 	//这里是新加的IP
	IPADDR3="172.16.1.3"
	TYPE=Ethernet
	PREFIX=16
	PREFIX2=16
	PREFIX3=16
	DEFROUTE=yes
	IPV4_FAILURE_FATAL=yes
	IPV6INIT=no
	NAME="System eth1"
	UUID=9c92fad9-6ecb-3e6c-eb4d-8a47c6f50c04
	HWADDR=00:0C:29:4E:63:EC 	//这里也很关键

## Ubuntu下配置单网卡双IP：

	sudo vi /etc/network/interfaces

	auto lo
	iface lo inet loopback

	# The primary network interface
	auto eth0
	iface eth0 inet static
	address 119.167.227.17
	netmask 255.255.255.192
	network 119.167.227.0
	broadcast 119.167.227.255
	gateway 119.167.227.62
	dns-nameservers 202.102.128.68
	dns-search workgroup

	# Virtual IP 01
	auto eth0:0
	iface eth0:0 inet static
	address 119.167.227.18
	netmask 255.255.255.192
	network 119.167.227.0
	broadcast 119.167.227.255
	gateway 119.167.227.62
	dns-nameservers 202.102.128.68
	dns-search workgroup

	# Virtual IP 02
	auto eth0:1
	iface eth0:1 inet static
	address 119.167.227.19
	netmask 255.255.255.192
	network 119.167.227.0
	broadcast 119.167.227.255
	gateway 119.167.227.62
	dns-nameservers 202.102.128.68
	dns-search workgroup

	# Virtual IP 03
	auto eth0:2
	iface eth0:2 inet static
	address 119.167.227.20
	netmask 255.255.255.192
	network 119.167.227.0
	broadcast 119.167.227.255
	gateway 119.167.227.62
	dns-nameservers 202.102.128.68
	dns-search workgroup

	# The second network interface
	auto eth1
	iface eth1 inet static
	address 192.168.1.2
	netmask 255.255.255.0
	network 192.168.1.0
	broadcast 192.168.1.255

以上是自己工作中的一个配置例子。亲测。