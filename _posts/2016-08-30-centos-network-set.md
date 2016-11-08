---
layout: post
title: CentOS 6网络配置
date: 2016-08-30
categories: blog
tags: [centos]
description: CentOS 6网络配置

---

## 常见的网络设备

lo 回环设备

eth0 系统内第一块以太网卡

ppp0 系统内第一个串行设备(多数出现在使用ADSL拨入Internet时)

### CentOS 6 配置网卡时有4种配置方式

可以跟据自己情况选择合适的一种，

这里以Centos 6.2为例，网卡以eth0为例

## 一、图形界面配置方式

打开“系统”——“首选项”——“网络连接”
选中要操作的网卡，点击编辑


先勾选左上角的“自动连接”,

选择IPv4设置，把[方法]选为“手动”,

然后点击“添加”，依次输入地址、子网掩码、网关，DNS;

我这里的配置为

    IP： 192.168.1.122
    子网掩码：255.255.255.0
    网关： 192.168.1.1
    DNS： 192.168.1.1

最后点击“应用”即可!

## 二、使用setup配置

打开终端，输入命令 

`setup`

打开配置工具，选择网络配置

我们先选择“设备配置”，稍后再配置DNS

选择要配置的网卡eth0

配置完点“确定”即可

接着返回配置DNS，最后保存退出


## 三、直接编辑配置文件

打开终端，输入命令： 

`vi /etc/sysconfig/network-scripts/ifcfg-eth0`

    DEVICE= 表示物理设备的名字
    ONBOOT= yes表示系统启动时激活该设备，no表示不激活
    BOOTPROTO= 取值可以是static(静态配置)、bootp(使用bootp协议)、dhcp(使用dhcp协议)
    BROADCAST= 表示广播地址
    IPADDR= 表示该网卡的IP地址
    PREFIX= 子网掩码
    GATEWAY=表示网关
    DNS*=表示DNS

## 四、命令行配置

### 1.配置eth0地址

输入命令：

`ifconfig eth0 192.168.1.122 netmask 255.255.255.0`

### 2.添加默认路由

输入命令：

`route add default gw 192.168.1.1`

### 3.配置DNS

输入命令：

`vi /etc/resolv.conf`

将 `nameserver 192.168.1.1`加入文本

配置完毕后重启网络服务

输入命令：

`service network restart`

### 查看网卡配置

输入命令：

`ifconfig eth0`

如果要查看所有网卡，则只用

`ifconfig`

即可


### 检查网卡状态

输入命令：

`ethtool eth0`

`Link detected:yes`，表示网卡已连接

### 检查网络连通性

输入命令：ping -c 4 baidu.com