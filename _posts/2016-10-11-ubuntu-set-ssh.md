---
layout: post
title: 让 Ubuntu 的 ssh 保持长时间连接
date: 2016-10-11
categories: blog
tags: [ubuntu]
description:  让 Ubuntu 的 ssh 保持长时间连接

---

> Ubuntu下的ssh连接老是自己会断，一段时间不理它就会失去响应，
> 
> 如何让ssh连接服务器或者ssh tunnel保持连接呢?

其实也很方便，打开 **/etc/ssh/ssh_config** 文件：

`sudo vi /etc/ssh/ssh_config`

添加两个参数：

    TCPKeepAlive yes
    ServerAliveInterval 300

说明：

前一个参数是说要保持连接，

后一个参数表示每过5分钟发一个数据包到服务器表示“我还活着”

如果你没有root权限，修改或者创建~/.ssh/ssh_config也是可以的
 