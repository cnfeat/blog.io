---
layout: post
title: Ubuntu Linux 下设置 IP 的配置命令
date: 2016-10-13
categories: blog
tags: [ubuntu]
description:  Ubuntu Linux 下设置 IP 的配置命令

---

## 一、使用命令设置ubuntu的ip地址

#### (1.修改配置文件blacklist.conf禁用IPV6：)

`sudo vi /etc/modprobe.d/blacklist.conf`

#### (2.在文档最后添加 blacklist ipv6，然后查看修改结果：)

`cat /etc/modprobe.d/blacklist.conf`

#### 3.设置IP（设置网卡eth0的IP地址和子网掩码）

`sudo ifconfig eth0 192.168.2.1 netmask 255.255.255.0`

#### 4.设置网关

`sudo route add default gw 192.168.2.254`

#### 5.设置DNS 修改/etc/resolv.conf

`sudo vi /etc/resolv.conf`

在其中加入

    nameserver DNS的地址1
    nameserver DNS的地址2 

#### 6.重启网络服务（若不行，请重启ubuntu：sudo reboot）：

`sudo /etc/init.d/networking restart`

#### 7.查看当前IP：

`ifconfig`

## 二、直接修改系统Ubuntu Linux配置文件

打开网络Ubuntu Linux配置文件是：

`sudo vi /etc/network/interfaces`

可设置DHCP或手动设置静态ip。前面auto eth0，让网卡开机自动挂载。

#### 1. 以DHCP方式配置网卡

编辑文件/etc/network/interfaces：

`sudo vi /etc/network/interfaces`

并用下面的行来替换有关eth0的行：

    # The primary network interface - use DHCP to find our address
    auto eth0
    iface eth0 inet dhcp

用下面的命令使网络设置生效：

`sudo /etc/init.d/networking restart`

也可以在命令行下直接输入下面的命令来获取地址

`sudo dhclient eth0`

#### 2. 为网卡配置静态IP地址

编辑文件/etc/network/interfaces：

`sudo vi /etc/network/interfaces`

并用下面的行来替换有关eth0的行：

    # The primary network interface
    auto eth0
    iface eth0 inet static
    address 192.168.2.1
    gateway 192.168.2.254
    netmask 255.255.255.0
    #network 192.168.2.0
    #broadcast 192.168.2.255

将上面的ip地址等信息换成你自己就可以了.用下面的命令使网络设置生效：

`sudo /etc/init.d/networking restart`

#### 3. 设定第二个IP地址(虚拟IP地址)

编辑文件/etc/network/interfaces：

`sudo vi /etc/network/interfaces`

在该文件中添加如下的行：

    auto eth0:1
    iface eth0:1 inet static
    address x.x.x.x
    netmask x.x.x.x
    network x.x.x.x
    broadcast x.x.x.x
    gateway x.x.x.x

根据你的情况填上所有诸如address,netmask,network,broadcast和gateways等信息，
用下面的命令使网络设置生效：

`sudo /etc/init.d/networking restart`

#### 4. 设置主机名称(hostname)

使用下面的命令来查看当前主机的主机名称：

`sudo /bin/hostname`

使用下面的命令来设置当前主机的主机名称：

`sudo /bin/hostname newname`

系统启动时,它会从 /etc/hostname 来读取主机的名称。

#### 5. Ubuntu Linux配置DNS

首先，你可以在/etc/hosts中加入一些主机名称和这些主机名称对应的IP地址，这是简单使用本机的静态查询。

要访问DNS 服务器来进行查询,需要设置/etc/resolv.conf文件，假设DNS服务器的IP地址是192.168.2.2, 

那么/etc/resolv.conf文件的内容应为：

`nameserver 192.168.2.2`

[《彻底解决Ubuntu 14.04 重启后DNS配置丢失的问题》](https://azraelgreen.github.io/blog/2016/09/30/ubuntu-set-dns/)

#### 6.手动重启网络服务：

`sudo /etc/init.d/networking restart`