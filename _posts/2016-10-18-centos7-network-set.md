---
layout: post
title: CentOS7 配置静态 IP 地址和 DNS
date: 2016-10-18
categories: blog
tags: [centos]
description: CentOS7 配置静态 IP 地址和 DNS

---

为你的 CentOS7 服务器配置静态 IP 地址、路由以及 DNS，我们会 **使用 ip 命令代替 ifconfig 命令**。

当然，ifconfig 命令对于大部分 Linux 发行版来说还是可用的，还能从默认库安装。

`# yum install net-tools`

[它提供 ifconfig 工具，如果你不习惯 ip 命令，还可以使用它]

但正如我之前说，我们会使用 ip 命令来配置静态 IP 地址。

### 首先检查了当前的 IP 地址。

`# ip addr show`

### 现在用你的编辑器打开并编辑配置文件

`# vi /etc/sysconfig/network-scripts/ifcfg-enp0s3`

我们会编辑文件中的四个地方。注意下面的四个地方并保证不碰任何其它的东西。也保留双引号，在它们中间输入你的数据。

    IPADDR = "[在这里输入你的静态 IP]"
    GATEWAY = "[输入你的默认网关]"
    DNS1 = "[你的DNS 1]"
    DNS2 = "[你的DNS 2]"

更改了 ‘ifcfg-enp0s3’ 之后，保存并退出。

    TYPE=Ethernet
    BOOTPROTO=static
    DEFROUTE=yes
    PEERDNS=yes
    PEERROUTES=yes
    IPV4_FAILURE_FATAL=no
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_PEERDNS=yes
    IPV6_PEERROUTES=yes
    IPV6_FAILURE_FATAL=no
    NAME=enp0s3
    UUID=5690e914-d613-4df2-85c2-b7de7864f2da
    DEVICE=enp0s3
    ONBOOT=yes
    IPADDR="192.168.16.220"
    GATEWAY="192.168.16.1"
    DNS1="192.168.16.1"
    DNS2="8.8.8.8"

### 重启网络服务并检查 IP 是否和分配的一样。

如果一切都顺利，用 Ping 查看网络状态。

`# service network restart`

重启网络后，确认检查了 IP 地址和网络状态。