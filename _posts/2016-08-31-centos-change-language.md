---
layout: post
title: CentOS 修改操作系统语言方法
date: 2016-08-31
categories: blog
tags: [centos]
description: CentOS 修改操作系统语言方法

---

## 一、CentOS7.0 

1、修改为中文

`# localectl  set-locale LANG=zh_CN.utf8`

2、修改为英文

`# localectl  set-locale LANG=en_US.UTF-8`

## 二、CentOS6.5

1、修改为英文

    # echo 'LANG=en_US.UTF-8' >>/etc/profile
    # source /etc/profile
    # su -l

2、修改为中文

    # echo 'LANG=zh_CN.utf8' >>/etc/profile
    # source /etc/profile
    # su -l
