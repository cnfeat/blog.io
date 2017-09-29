---
layout: post
title: linux下如何查看crontab的日志记录
date: 2017-06-12
categories: blog
tags: [linux]
description: linux下如何查看crontab的日志记录
---

> 有时候我们会发现，`crontab` 中的计划任务时而成功，时而不成功，什么原因呢？定位问题的关键点，就要通过日志来分析。

### 一、Linux 系统下


查看 `/var/log/cron` 这个文件就可以，可以用

	tail -f /var/log/cron  # 实时查看最新日志内容

或者

	less /var/log/cron  # 查看全部日志内容

### 二、Unix 系统下


在 `/var/spool/cron/tmp` 文件中，有 `croutXXX001864` 的 tmp 文件，tail 这些文件

	tail -f /var/spool/cron/tmp/croutXXX001864.tmp

就可以看到正在执行的任务了。

### 三、mail任务

在 `/var/spool/mail/root` 文件中，有 `crontab` 执行日志的记录，用

	tail -f /var/spool/mail/root

即可查看最近的 `crontab` 执行情况。
