---
layout: post
title: centos 7 使用 163 yum 源
date: 2016-10-19
categories: blog
tags: [centos]
description: centos 7 使用 163 yum 源

---

一般是下载 .repo 源即可，但有时候我们需要安装一些额外的包，就需要下载 Extra Packages for Enterprise Linux (EPEL) 源， 比如我们需要用 yum 安装 ansible ，就需要安装 epel 。

### 下载 repo 源

repo 源一般包括 base, updates, Extras 三部分。 

`$ cd /etc/yum.repos.d/ `

`$ wget http://mirrors.163.com/.help/CentOS7-Base-163.repo` 

如果还有其他源，可以暂且把他们重命名成其他扩展名，比如 centos-base.repo.bak 

`$ mv CentOS7-Base.repo CentOS7-Base.repo.bak`

    $ mv CentOS7-Base-163.repo CentOS7-Base.repo 
    $ yum clean all 
    $ yum makecache

### 安装 epel 源。

    $ yum install epel-release 
    $ yum makecache

### 检查是否安装成功

`$ yum repolist`

从输出可以看出，我们安装了 repo 的三个部分 base, updates, Extras. 而 EPEL 则安装了 epel 部分。