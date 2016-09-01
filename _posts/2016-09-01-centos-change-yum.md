---
layout: post
title: CentOS6.5 中修改 yum 源方法
date: 2016-09-01
categories: blog
tags: [centos]
description: CentOS6.5 中修改 yum 源方法

---

在安装完CentOS后一般需要修改yum源，才能够在安装更新rpm包时获得比较理想的速度。

国内比较快的有163源、sohu源。这里以163源为例子。

先确认wget安装了没有，如果是minimal版本，默认是没装的！

#### 安装 wget : 

`yum install wget`

#### 修改 yum 源：

    cd /etc/yum.repos.d
    mv CentOS-Base.repo CentOS-Base.repo.backup
    wget http://mirrors.163.com/.help/CentOS6-Base-163.repo
    mv CentOS6-Base-163.repo CentOS-Base.repo
    yum clean all 
