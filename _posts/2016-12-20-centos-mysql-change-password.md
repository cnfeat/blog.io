---
layout: post
title: 修改 mysql 用户密码方法
date: 2016-12-20
categories: blog
tags: [mysql]
description: 修改 mysql 用户密码方法
---

### 一、mysqladmin命令（回目录）

格式如下（其中，USER为用户名，PASSWORD为新密码）：

`mysqladmin -u USER -p password PASSWORD`

该命令之后会提示输入原密码，输入正确后即可修改。

例如，设置root用户的密码为123456，则

`mysqladmin -u root -p password 123456`

### 二、UPDATE user 语句（回目录）

这种方式必须是先用root帐户登入mysql，然后执行：

    UPDATE user SET password=PASSWORD('123456') WHERE user='root';
    FLUSH PRIVILEGES;

### 三、SET PASSWORD 语句（回目录）

这种方式也需要先用root命令登入mysql，然后执行：

`SET PASSWORD FOR root=PASSWORD('123456');`