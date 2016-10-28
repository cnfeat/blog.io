---
layout: post
title: vsftpd 的主动模式与被动模式
date: 2016-10-28
categories: blog
tags: [ftp]
description: vsftpd 的主动模式与被动模式

---

## FTP两种模式的区别：

### （1）PORT（主动）模式

所谓主动模式，指的是FTP服务器“主动”去连接客户端的数据端口来传输数据，

其过程具体来说就是：客户端从一个任意的非特权端口N（N>1024）连接到FTP服务器的命令端口（即tcp 21端口），紧接着客户端开始监听端口N+1，并发送FTP命令“port N+1”到FTP服务器。然后服务器会从它自己的数据端口（20）“主动”连接到客户端指定的数据端口（N+1），这样客户端就可以和ftp服务器建立数据传输通道了。

### （2）PASV（被动）模式

所谓被动模式，指的是FTP服务器“被动”等待客户端来连接自己的数据端口，

其过程具体是：当开启一个FTP连接时，客户端打开两个任意的非特权本地端口（N >1024和N+1）。第一个端口连接服务器的21端口，但与主动方式的FTP不同，客户端不会提交PORT命令并允许服务器来回连它的数据端口，而是提交PASV命令。这样做的结果是服务器会开启一个任意的非特权端口（P > 1024），并发送PORT P命令给客户端。然后客户端发起从本地端口N+1到服务器的端口P的连接用来传送数据。

**（注意此模式下的FTP服务器不需要开启tcp 20端口了）**

## 两种模式的比较：

（1）PORT（主动）模式模式只要开启服务器的21和20端口，而PASV（被动）模式需要开启服务器大于1024所有tcp端口和21端口。

（2）从网络安全的角度来看的话似乎ftp PORT模式更安全，而ftp PASV更不安全，那么为什么RFC要在ftp PORT基础再制定一个ftp PASV模式呢？

其实RFC制定ftp PASV模式的主要目的是为了数据传输安全角度出发的，因为ftp port使用固定20端口进行传输数据，那么作为黑客很容使用sniffer等探嗅器抓取ftp数据，这样一来通过ftp PORT模式传输数据很容易被黑客窃取，因此使用PASV方式来架设ftp server是最安全绝佳方案。

因此：如果只是简单的为了文件共享，完全可以禁用PASV模式，解除开放大量端口的威胁，同时也为防火墙的设置带来便利。

不幸的是，**FTP工具或者浏览器默认使用的都是PASV模式连接FTP服务器**，因此，必须要使vsftpd在开启了防火墙的情况下，也能够支持PASV模式进行数据访问。

## 如何开启vsftpd的PASV模式？

### （1）修改 /etc/vsftpd/vsftpd.conf 文件配置

    pasv_enable=yes （Default: YES） 设置是否允许pasv模式
    #pasv_promiscuous=no （Default: NO） 是否屏蔽对pasv进行安全检查，（当有安全隧道时可禁用）
    pasv_max_port=10240 （Default: 0 (use any port)） pasv使用的最大端口
    pasv_min_port=20480 （Default: 0 (use any port)） pasv使用的最小端口

默认情况下vsftpd是支持PASV模式访问的，可以不作修改。

### （2）给防火墙添加FTP访问转换支持模块

`#vi /etc/sysconfig/iptables-config`

添加以下两行：

    IPTABLES_MODULES="ip_conntrack_ftp"
    IPTABLES_MODULES="ip_nat_ftp"

请一定注意两行内容的位置关系不要搞反了。如果将"ip_nat_ftp"放到前面是加载不到的。

如果你的ftp服务是过路由或者防火墙（即内网映射方式一定需要此模块）。以上等同于在加载iptables之前运行modprobe命令加载"ip_nat_ftp"和"ip_conntrack_ftp"模块。

### （3）给防火墙添加访问规则允许(开放被动模式访问端口 10240 至 20480 )

`-A INPUT -m state --state NEW -m tcp -p tcp --dport 10240:20480 -j ACCEPT`

### （4）重启防火墙服务

`service iptables restart`

### （5）检查模块是否加载成功

    #lsmod |grep ftp
    
    p_nat_ftp  7361  0 
    ip_nat 21229  1 ip_nat_ftp
    ip_conntrack_ftp   11569  1 ip_nat_ftp
    ip_conntrack   53665  4 ip_nat_ftp,ip_nat,ip_conntrack_ftp,xt_state

以上信息表明模块加载成功，可以在其他机器上使用FTP客户端进行访问了。

