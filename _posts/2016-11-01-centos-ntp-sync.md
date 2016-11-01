---
layout: post
title: ntp内网时间同步（基于centos6.5）
date: 2016-11-01
categories: blog
tags: [centos]
description: ntp内网时间同步（基于centos6.5）

---

## 服务器端：

安装ntp

`yum install ntp`

编辑配置文件

`vi /etc/ntp.conf`

添加如下内容：(允许 192.168.0 网段的客户端连接)

    restrict 192.168.0.0 mask 255.255.255.0 nomodify notrap
    server time-b.nist.gov

其他server都注释掉

`vi /etc/sysconf/iptables`

添加iptables规则

`-A  INPUT -m state --state NEW -m udp -p udp --dport 123 -j ACCEPT`

重启iptables

`sudo service iptables restart`

重启ntp

`sudo service ntpd restart`

## 客户端：

安装ntp

`yum install ntp`

编辑配置文件

`vi /etc/ntp.conf`

添加

    server 192.168.0.111（配置的ntp服务器端IP地址）

其他server都注释掉

同步时间

`ntpdate 192.168.0.111`

把同步命令添加到任务计划

`crontab -e`

查看时间

`date`

此客户端时间同步配置完成。