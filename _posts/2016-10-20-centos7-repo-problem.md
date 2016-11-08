---
layout: post
title: CentOS 7 提示'cannot find a valid baseurl for repo'处理办法
date: 2016-10-20
categories: blog
tags: [centos]
description: CentOS 7 提示'cannot find a valid baseurl for repo'处理办法

---

### 问题：

用的是最小化安装的CentOS 7，当运行 yum update 时，提示 “cannot find a valid baseurl for repo” 错误提示。

### 处理办法：

网上搜索后说是 dns 的问题，可能没有自动到 IP 地址导致。

运行：

`dhclient`