---
layout: post
title: ssh登录很慢但登录成功后速度正常的问题分析
date: 2017-03-15
categories: blog
tags: [ssh]
description: ssh登录很慢但登录成功后速度正常的问题分析
---

可能的原因有以下两点：

### 一、DNS反向解析的问题

OpenSSH在用户登录的时候会验证IP，它根据用户的IP使用反向DNS找到主机名，再使用DNS找到IP地址，最后匹配一下登录的IP是否合法。如果客户机的IP没有域名，或者DNS服务器很慢或不通，那么登录就会很花时间。

#### 解决办法：

只需修改 `/etc/ssh/sshd_config`，设置 `UseDNS` 为 `no` 即可（如果没有这个选项，就直接添加一个进去）：

`sed -i "s/#UseDNS yes/UseDNS no/"  /etc/ssh/sshd_config`

注：Remote protocol version 2.0, remote software version OpenSSH_5.3。以上版本配置文件里没有发现这个选项


### 二、gssapi的问题

用 `ssh -v user@server` 可以看到登录时有如下信息：

debug1: Next authentication method: gssapi-with-mic
debug1: Unspecified GSS failure.  Minor code may provide more information

#### 解决办法：

可以使用 `ssh  -o GSSAPIAuthentication=no user@server` 登录，
也可以修改 `/etc/ssh/ssh_config`，设置 `GSSAPIAuthentication no`
 
最后 `service sshd restart`


注：在连接服务器输入 `ssh -v 192.168.1.20` 使用 `-v` 这个选项，可以看到整个连接的过程，可以知道是哪个步骤慢了。