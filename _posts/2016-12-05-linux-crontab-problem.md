---
layout: post
title: crontab command not found 解决办法
date: 2016-12-05
categories: blog
tags: [linux]
description: crontab command not found 解决办法
---

crontab命令是大多数系统都有的命令，不排除由于有些系统被精简而没有的，此时在命令行中运行crontab会提示：

`crontab: command not found`

### 对于CentOS系统，我们可以通过下面的命令来安装：

`yum -y install  vixie-cron crontabs`

vixie-cron 软件包是cron的主程序；

crontabs 软件包是用来安装、卸装、或列举用来驱动 cron 守护进程的表格的程序。

### 网上找到的另一种安装方法是：

`# yum install cronie`

安装完成后，使用命令

`crontab -e`

即可编辑 crontab，添加定时任务