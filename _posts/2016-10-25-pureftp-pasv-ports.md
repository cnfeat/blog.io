---
layout: post
title: centos 系统下，FTP 服务器开启被动模式端口
date: 2016-10-25
categories: blog
tags: [centos,ftp]
description: FTP 服务器开启被动模式端口

---

配置了PureFTP上传程序，在本地使用 FlashFTP 时出现最多的“数据 Socket 错误 连接已超时”错误，无论使用主动还是被动模式上传，都会有类似错误！ 
 
    [右] 数据 Socket 错误: 没有到主机的通道
    [右] 列表 错误
    [右] PASV
    [右] 227 Entering Passive Mode (116,255,246,176,83,197)
    [右] 正在打开数据连接 IP: xxx.xxx.xxx.xxx 端口: 21445
    [右] 数据 Socket 错误: 连接已超时
    [右] 列表 错误
    [右] 以 PASV 模式连接失败，正在尝试使用 PORT 模式。
    [右] 侦听于端口: 4447，正在等候连接。
    [右] PORT 192,168,1,222,17,95
    [右] 500 我不能开启连接到 xxx.xxx.xxx.xxx (仅 xxx.xxx.xxx.xxx)
    [右] 列表 错误
    [右] QUIT
 
可能是CentOS防火墙规则的问题，因为FTP能连接到主机，但无法显示列表！
 
查看防火墙规则：

`# vi /etc/sysconfig/iptables`

其中关于FTP的有如下两条：

    -A INPUT -p tcp -m state --state NEW -m tcp --dport 20 -j ACCEPT
    -A INPUT -p tcp -m state --state NEW -m tcp --dport 21 -j ACCEPT
 
应该不会有错了，又翻查了一下错误信息，发现被动模式的端口总是无法连接上，灵光一闪，查看 PureFTP 配置文件里的被动模式端口号段： 
 
    # vi /usr/local/pureftpd/pure-ftpd.conf
    PassivePortRange  10000 20000
 
iptables 里加了一条

`-A INPUT -m state --state NEW -m tcp -p tcp --dport 10000:20000 -j ACCEPT`

PS：如何使用的 FTP 服务是 vsftpd ，也是一样的原理，需要配置并打开被动模式端口。