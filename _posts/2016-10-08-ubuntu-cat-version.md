---
layout: post
title: 如何查看当前 Ubuntu 系统的版本
date: 2016-10-08
categories: blog
tags: [ubuntu]
description: 如何查看当前 Ubuntu 系统的版本

---

### 查看当前Linux系统的版本的方法

#### 第一种

使用命令：

`cat /proc/version `

#### 第二种

使用命令：

`uname -a`

#### 第三种

使用命令：

`lsb_release -a` 


> 注：proc 目录下记录的当前系统运行的各种数据，version 记录的版本信息直接可以通过 cat 查看到。

    