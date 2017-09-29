---
layout: post
title: Ubuntu 出现 xxx is not in the sudoers file 解决方法
date: 2016-10-12
categories: blog
tags: [ubuntu]
description:  Ubuntu 出现 xxx is not in the sudoers file 解决方法

---

Ubuntu 系统里，在一般用户下执行 sudo 命令提示：

`xxx is not in the sudoers file. This incident will be reported` 

解决方法：

### 一、找出文件所在的位置

`$whereis sudoers`

默认都是 **/etc/sudoers**

### 二、查看原文件权限

`ls -al /etc/sudoers`

### 三、修改文件权限(给当前用户添加修改此文件的权限)

`#chmod u+w /etc/sudoers`    

以超级用户登录:

`su root`

### 四、编辑文件

`vi /etc/sudoers`

在 root ALL=(ALL)ALL 行下添加：

`XXX ALL=(ALL)ALL  # XXX为你的用户名`

### 最后，修改回文件的原权限：

`#chmod u－w /etc/sudoers`

