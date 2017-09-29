---
layout: post
title: 查看 centos 中的用户和用户组
date: 2016-09-09
categories: blog
tags: [centos]
description: 查看 centos 中的用户和用户组

---

#### 用户列表文件：

/etc/passwd

#### 用户组列表文件：

/etc/group

#### 查看系统中有哪些用户：

`cut -d : -f 1 /etc/passwd`

#### 查看可以登录系统的用户：

`cat /etc/passwd | grep -v /sbin/nologin | cut -d : -f 1`

#### 查看用户操作：

`w 命令(需要root权限)`

#### 查看某一用户：

`w 用户名`

#### 查看登录用户：

`who`

#### 查看用户登录历史记录：

`last`

#### 查看用户的所在组号，用户号：

`id 用户名`