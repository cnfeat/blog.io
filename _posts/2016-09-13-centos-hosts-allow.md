---
layout: post
title: CentOS下ssh登录限制ip的方法
date: 2016-09-13
categories: blog
tags: [centos]
description: CentOS下ssh登录限制ip的方法

---

## CentOS下ssh登录限制ip的方法 及 etc-hosts.allow 和-etc-hosts.deny 详解

/etc/hosts.allow（允许）和/etc/hosts.deny（禁止）这两个文件是tcpd服务器的配置文件。
是控制远程访问设置的，通过他可以允许或者拒绝某个ip或者ip段的客户访问linux的某项服务。 

比如SSH服务，我们通常只对管理员开放，那我们就可以禁用不必要的IP，而只开放管理员可能使用到的IP段
 
 
### 方法一：

修改 **/etc/hosts.allow** 文件

`vi /etc/hosts.allow`

添加如下语句：

    sshd:192.168.0.251:allow
    sshd:222.77.15.*:allow

以上写法，第一条表示允许单个IP，第二条表示允许222这个ip段连接sshd服务（需要hosts.deny这个文件配合使用），

当然:allow完全可以省略的。

**/etc/hosts.deny** 文件，此文件是拒绝服务列表。

添加如下语句：

`sshd:all:deny`

语句 sshd:all:deny 表示拒绝了所有sshd远程连接。:deny可以省略。

注意修改完后：

`service xinetd restart`

才能让刚才的更改生效。

**注：当hosts.allow和 host.deny相冲突时，以hosts.allow设置为准。**

### 方法二：

`sshd:all:deny`

也完全可以直接写在 **/etc/hosts.allow** 里面，比如：

     sshd:192.168.0.251:allow
     sshd:222.77.15.*:allow
     sshd:all:deny

这样就只需要修改一个配置文件，更加简洁，个人更推荐这种方式。

此方法同样可引申到 Vfstpd 服务，

在 **/etc/hosts.allow** 文件添加如下语句：

    vsftpd:192.168.0.251:allow
    vsftpd:222.77.15.*:allow
    vsftpd:all:deny