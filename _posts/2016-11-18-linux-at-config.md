---
layout: post
title: at 命令实现定时任务
date: 2016-11-18
categories: blog
tags: [linux]
description: at 命令实现定时任务

---

### at 命令简介

假如我们只是想要让特定任务运行一次,那么，这时候就可以用at监控程序。

at类似打印进程，会把任务放到/var/spool/at目录中，到指定时间运行它 。

at命令相当于另一个shell，运行 `at time` 命令时，它发送一个个命令，可以输入任意命令或者程序。
 
### at命令执行流程:

    # at 2:05 tomorrow
    at>/home/do_job
    at> Ctrl+D
 
### AT Time 中的时间表示方法
 
    时间       例子                 说明
 
    Minute    at now + 5 minutes   任务在5分钟后运行
    Hour      at now + 1 hour      任务在1小时后运行
    Days      at now + 3 days      任务在3天后运行
    Weeks     at now + 2 weeks     任务在两周后运行
    Fixed     at midnight          任务在午夜运行
    Fixed     at 10:30pm           任务在晚上10点30分
 
注意：at命令的服务，linux其他版本默认为不启动，而ubuntu默认为启动的。

#### 显示语法:

`service atd`

#### 检查atd的状态:

 `service atd status`

#### 启动atd服务:

`service atd start`
 
### 查看at执行的具体内容：

一般位于 **/var/spool/at** 目录下面， 用 vi 打开，在最后一部分就是你的执行程序

#### 查询at已建立的计划

`atq`

#### 删除任务（先查询at计划，找出序号）

`atrm （数字序号）`

`atrm 2`

#### 举例：

三天后的下午 5 点锺执行 /bin/ls :

`at 5pm 3 days /bin/ls`
 
三个星期后的下午 5 点锺执行 /bin/ls :

`at 5pm 2 weeks /bin/ls`
 
明天的 17:20 执行 /bin/date :

`at 17:20 tomorrow /bin/date`
 
1999 年的最后一天的最后一分钟印出 the end of world !

`at 23:59 12/31/1999 echo "the end of world !" `