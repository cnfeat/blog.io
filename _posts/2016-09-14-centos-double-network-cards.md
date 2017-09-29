---
layout: post
title: CentOS 6.5 双网卡配置一个上外网一个接局域网
date: 2016-09-14
categories: blog
tags: [centos]
description: CentOS 6.5 双网卡配置一个上外网一个接局域网

---

### 1、配置DNS

修改对应网卡的DNS的配置文件

`# vi /etc/resolv.conf `

修改以下内容，可以设置多个：

    nameserver 202.106.0.20
    nameserver 114.114.114.114

### 2、配置外网网卡的网关 

修改网关的配置文件

`[root@centos]# vi /etc/sysconfig/network`

修改以下内容

    NETWORKING=yes    (表示系统是否使用网络，一般设置为yes。如果设为no，则不能使用网络，而且很多系统服务程序将无法启动)
    HOSTNAME=centos    (设置本机的主机名，这里设置的主机名要和/etc/hosts中设置的主机名对应)
    GATEWAY=192.168.1.1    (注意：这里需要配置为外网网卡的网关)

### 3、配置IP地址

修改对应网卡的IP地址的配置文件

`# vi /etc/sysconfig/network-scripts/ifcfg-eth0`

修改以下内容

    DEVICE=eth0       #描述网卡对应的设备别名，例如ifcfg-eth0的文件中它为eth0，Dell服务器的一般为：em1、em2
    BOOTPROTO=static      #设置网卡获得ip地址的方式，可能的选项为static，dhcp或bootp，分别对应静态指定的 ip地址，通过dhcp协议获得的ip地址，通过bootp协议获得的ip地址
    BROADCAST=192.168.0.255     #对应的子网广播地址
    HWADDR=00:07:E9:05:E8:B4     #对应的网卡物理地址
    IPADDR=12.168.1.2     #如果设置网卡获得 ip地址的方式为静态指定，此字段就指定了网卡对应的ip地址
    GATEWAY=     #注意：内网网卡这里必须为空，否则两个网卡同时启用后上不了外网
    IPV6INIT=no
    IPV6_AUTOCONF=no
    NETMASK=255.255.255.0     #网卡对应的网络掩码
    NETWORK=192.168.1.0     #网卡对应的网络地址
    ONBOOT=yes     #系统启动时是否设置此网络接口，设置为yes时，系统启动时激活此设备

### 4、重新启动网络配置

`# service network restart `

或

`# /etc/init.d/network restart`
