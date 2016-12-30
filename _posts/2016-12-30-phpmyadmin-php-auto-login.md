---
layout: post
title: phpMyAdmin自动登录和取消自动登录的配置方法
date: 2016-12-30
categories: blog
tags: [mysql]
description: phpMyAdmin自动登录和取消自动登录的配置方法
---

### 一、如何设置phpMyAdmin自动登录？

首先在根目录找到config.sample.inc.php

复制一份文件名改为 **config.inc.php** （如果已经存在 config.inc.php 文件，则直接修改该文件即可）。

打开**config.inc.php** 

找到 `$cfg['Servers'][$i]['auth_type']`，

将 `$cfg['Servers'][$i]['auth_type'] = 'cookie';`

改成

`$cfg['Servers'][$i]['auth_type'] = 'config';`

然后在下面追加如下代码：

    $cfg['Servers'][$i]['user']          = 'root';      // 设置的mysql用户名
    $cfg['Servers'][$i]['password']      = '123456';    // 设置的mysql密码

### 二、如何取消phpMyAdmin自动登录？

只需把

`$cfg['Servers'][$i]['auth_type'] = 'config';`

改成

`$cfg['Servers'][$i]['auth_type'] = 'cookie';`

保存即可。

### 备注：

`$cfg['Servers'][$i]['auth_type']` 有三个待选项值，即 cookie、http、config。

用的比较多的是 cookie与config。

当在正式环境时，用 cookie，要求用户必须输入正确的用户名与密码，

而在本地测试服务器时，一般用 config，省得session失效后又得输入用户名与密码，以节省开发时间。