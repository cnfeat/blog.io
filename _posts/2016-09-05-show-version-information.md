---
layout: post
title: 查看CentOS的CPU内存信息及操作系统的版本信息
date: 2016-09-05
categories: blog
tags: [centos]
description: 查看CentOS的CPU内存信息及操作系统的版本信息

---

## 因工作需要，要经常查看Linux服务器的CPU，内存信息以及操作系统版本等信息，记录在此作为备忘。常用的命令如下：

### 1.查看CPU型号（8个逻辑CPU）

    [[root@mail ~]# cat /proc/cpuinfo |grep "name" |cut -f2 -d: |uniq -c

    8 Intel(R) Xeon(R) CPU   E5506  @ 2.13GHz

### 2.查看物理CPU个数（两个4核CPU）

    [root@mail ~]# cat /proc/cpuinfo | grep "physical"| sort |uniq -c

    8 address sizes: 40 bits physical, 48 bits virtual
    4 physical id: 0
    4 physical id: 1

### 3.查看CPU运行在多少位模式下面

    [root@mail ~]# getconf LONG_BIT

    64

### 4.查看cpu是否支持64位
查看cpuinfo中是否有lm，如果有lm表示支持64位，lm的意思是long mode。下面的结果大于0，说明支持64位操作系统

    [root@mail ~]# cat /proc/cpuinfo | grep flags | grep ' lm ' | wc -l

    8

### 5.查看系统物理内存的大小

    [root@mail ~]# free

    total   used   free sharedbuffers cached
    Mem:  8168144   7387980 780164  0 6898564014308
    -/+ buffers/cache:26838165484328
    Swap:  4192924  244244168500

### 6.查看 CPU 详细信息

    [root@mail ~]# cat /proc/cpuinfo

### 7.查看服务器安装的是哪个发行版本

    [root@mail ~]# cat /etc/redhat-release

    CentOS release 5.9 (Final)   （70.6）

### 8.查看OS的版本是64位的还是32位的

    [root@mail ~]# uname -a

    Linux mail 2.6.18-164.el5 #1 SMP Thu Sep 3 03:28:30 EDT 2009 x86_64 x86_64 x86_64 GNU/Linux

### 9.查看服务器的硬件信息

    [root@DB145]# dmidecode |grep Vendor //处理器厂商

       Vendor: Dell Inc.

    [root@DB145]# dmidecode |grep Product //服务器型号

       Product Name: PowerEdge R610
       Product Name: 0RP59R
