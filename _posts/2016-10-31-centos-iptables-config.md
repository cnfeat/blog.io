---
layout: post
title: CentOS 6上 iptables 防火墙的基本配置教程
date: 2016-10-31
categories: blog
tags: [centos,iptables]
description: CentOS 6上 iptables 防火墙的基本配置教程

---

## 在 CentOS 6 中设置防火墙，使用 iptables 命令添加规则：

### 1、安装 iptables 防火墙

如果没有安装 iptables 需要先安装，CentOS 执行：

`yum install iptables`

### 2、清除已有iptables规则

    iptables -F
    iptables -X
    iptables -Z

备注：

运行 `iptables -L -n`，如果内容为空，则下二条可以不运行

### 3、开放指定的端口

**-A 和 -I** 参数分别为添加到 **规则末尾 和 规则最前面** 。

    #允许本地回环接口(即运行本机访问本机)
    iptables -A INPUT -i lo -j ACCEPT
    # 允许已建立的或相关连的通行
    iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    #允许所有本机向外的访问
    iptables -A OUTPUT -j ACCEPT
    # 允许访问22端口
    iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    #允许访问80端口
    iptables -A INPUT -p tcp --dport 80 -j ACCEPT
    #允许FTP服务的21和20端口
    iptables -A INPUT -p tcp --dport 21 -j ACCEPT
    iptables -A INPUT -p tcp --dport 20 -j ACCEPT
    #如果有其他端口的话，规则也类似，稍微修改上述语句就行
    #允许ping
    iptables -A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT
    #禁止其他未允许的规则访问
    iptables -A INPUT -j REJECT  #（注意：如果22端口未加入允许规则，SSH链接会直接断开。）
    iptables -A FORWARD -j REJECT

### 4、屏蔽IP

    #如果只是想屏蔽IP的话 “3、开放指定的端口” 可以直接跳过。
    #屏蔽单个IP的命令是
    iptables -I INPUT -s 123.45.6.7 -j DROP
    #封整个段即从123.0.0.1到123.255.255.254的命令
    iptables -I INPUT -s 123.0.0.0/8 -j DROP
    #封IP段即从123.45.0.1到123.45.255.254的命令
    iptables -I INPUT -s 124.45.0.0/16 -j DROP
    #封IP段即从123.45.6.1到123.45.6.254的命令是
    iptables -I INPUT -s 123.45.6.0/24 -j DROP

### 5、查看已添加的iptables规则

`iptables -L -n`

*v：显示详细信息，包括每条规则的匹配包数量和匹配字节数*

*x：在 v 的基础上，禁止自动单位换算（K、M）*

*n：只显示IP地址和端口号，不将ip解析为域名*

### 6、删除已添加的iptables规则

将所有 iptables 以序号标记显示，执行：

`iptables -L -n --line-numbers`

比如要删除 INPUT 里序号为 8 的规则，执行：

`iptables -D INPUT 8`

### 7、iptables 的开机启动及规则保存

设置 iptables 开机自启动：

`chkconfig --level 345 iptables on`

保存规则：

`service iptables save`  


## 在CentOS 6 中直接编辑配置文件，添加 iptables 防火墙规则：

iptables 的配置文件为 **/etc/sysconfig/iptables** 

编辑配置文件：

`vi /etc/sysconfig/iptables`

文件中的配置规则与通过iptables命令配置，语法相似：

如，通过iptables命令配置，允许访问80端口：

`iptables -A INPUT -p tcp --dport 80 -j ACCEPT`

那么，在文件中配置，只需要去掉句首的 iptables ，添加如下内容：

`-A INPUT -p tcp --dport 80 -j ACCEPT`

保存退出。

重启iptables服务使其生效：

`service iptables restart`

## 后记

关于更多的 iptables 的使用方法可以执行：

`iptables --help`

或网上搜索一下 iptables 参数的说明。

另 新版的 CentOS 7 的防火墙使用 firewalld 来配置，与 CentOS 6 配置方法有很大不同。
