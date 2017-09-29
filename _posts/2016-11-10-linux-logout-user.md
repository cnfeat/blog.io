---
layout: post
title: Linux 注销在线用户方法
date: 2016-11-10
categories: blog
tags: [linux]
description: Linux 注销在线用户方法

---

在 Windows 里可以通过任务管理器注销掉某个在线用户。Linux 里也有类似方法，即使用 pkill 命令，步骤如下：
 
### 1. 先用 w 命令查看当前登录系统的用户：

```
[root@CentOS ~]# w
 11:48:09 up  3:13,  2 users,  load average: 0.00, 0.01, 0.00
 USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
 root     pts/0    218.17.167.82    11:47    0.00s  0.03s  0.01s w
 root     pts/1    218.17.167.82    11:43    2:34   0.03s  0.03s -bash
```
  
### 2. 使用 pkill 命令将从 pts/1 终端登录的用户注销：

`[root@centos ~]# pkill -kill -t pts/1`

其中第 1 个参数 -kill 中的 kill 是 SIGKILL 信号的缩写。-t 选项后跟着连接终端的名称
