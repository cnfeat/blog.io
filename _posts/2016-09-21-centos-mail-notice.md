---
layout: post
title: Centos关闭提示信息"You have new mail in /var/spool/mail/root"
date: 2016-09-21
categories: blog
tags: [centos]
description: Centos关闭提示信息"You have new mail in /var/spool/mail/root"

---

最近用centos，总是提示You have new mail in /var/spool/mail/root

### 如何不用关闭sendmail服务，直接取消提示的方法：

    [root@test]# echo "unset MAILCHECK" >> /etc/profile
    [root@test]# source /etc/profile

### 停止postfix邮件服务并卸载这个包方法：

以Centos为例：

    #service postfix stop
    #yum -y remove postfix

实际环境中不推荐这样做。用下面的较好，因为Crontab依托Postfix。

    #service postfix stop
    #chkconfig postfix off
