---
layout: post
title: VirtualBox 上 Ubuntu Server 虚拟机配置双网卡
date: 2017-03-24
categories: blog
tags: [virtualbox]
description: VirtualBox 上 Ubuntu Server 虚拟机配置双网卡
---

VirtualBox 上 Ubuntu Server 虚拟机需要同时访问内网和外网，但是特殊原因不能用桥接模式，所以就需要为虚拟机配置双网卡，一个分配外网，另一个分配内网。

#### 一、在 VirtualBox 的虚拟机设置里，添加两块网卡，第一块网卡使用 NAT 模式，第二块网卡使用 Host-Only 模式。

#### 二、启动虚拟机，使用 `ifconfig` 查看网络配置只能看到第一个网卡信息 eth0。

此时查看网络配置文件 `/etc/network/interfaces`，内容如下：

	auto lo
	iface lo inet loopback
 
	auto eth0
	iface eth0 inet dhcp

#### 三、修改此文件，添加 eth1 的配置信息

	auto eth1
	iface eth1 inet dhcp

或者

	auto eth1
	iface eth1 inet static
	address 192.168.56.101  # 参考宿主机器配置
	netmask 255.255.255.0

#### 四、重新启动虚拟机然后使用 `ifconfig` 查看网络情况。