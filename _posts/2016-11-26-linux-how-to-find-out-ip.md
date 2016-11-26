---
layout: post
title: 找出日志中访问量最大的 IP
date: 2016-11-26
categories: blog
tags: [linux]
description: 找出日志中访问量最大的 IP
---

### 目标：

现有一段apache的日志，需要从日志中提取出访问量最大的IP。使用shell实现。

### 步骤解析：

日志如下（只是举例，故数据量较小）：

    $ more aa.txt 

    127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326
    192.168.1.100 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326
    192.168.1.100 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326
    192.168.1.100 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326

### 1.要提取访问量最大的IP，需要先从日志中把IP段提取出来。

    $ cat aa.txt |awk -F " " '{print $1}'

    127.0.0.1
    192.168.1.100
    192.168.1.100
    192.168.1.100

PS，此处也可以用cut命令实现：

    $ cut -d " " -f 1 aa.txt 

    127.0.0.1
    192.168.1.100
    192.168.1.100
    192.168.1.100

### 2.对IP进行统计，看各IP出现过多少次

    $ cat aa.txt |awk -F " " '{print $1}' |uniq -c

    1 127.0.0.1
    3 192.168.1.100

（PS：wc -l 也可以对行数统计，但统计的是整体的，所有行数。不会分类统计）

### 3.按IP出现次数从大到小排列

    $ cat aa.txt |awk -F " " '{print $1}' |uniq -c |sort -r

    3 192.168.1.100
    1 127.0.0.1

特别提醒：

因样例中IP地址是连续出现的，所以可以这样统计。标准方法应该是先sort后uniq，避免相同IP地址不连续的时候被统计多次。

### 4.再次提取出IP段

    $ cat aa.txt |awk -F " " '{print $1}' |uniq -c |sort -r |awk '{print $2}'

    192.168.1.100
    127.0.0.1

### 5.选择第一行

    $ cat aa.txt |awk -F " " '{print $1}' |uniq -c |sort -r |awk '{print $2}' |head -1

    192.168.1.100

这样就把出现次数最多的IP找出来了。