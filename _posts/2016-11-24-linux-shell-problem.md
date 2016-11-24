---
layout: post
title: Linux Shell 按 Tab 键不能补全，方向键不能用
date: 2016-11-24
categories: blog
tags: [linux]
description: Linux Shell 按 Tab 键不能补全，方向键不能用

---

### 问题：

今天在 Linux 上用 useradd 新增用户的时候，发现使用新增的用户登陆的时候，在Shell里面不能使用 Tab 键补全命令，按上下键也不能切换历史命令，出现乱码的现象。而 Root 用户是正常的。

### 原因：

后面发现，在 /etc/passwd 里面，新增用户用的 Shell 与 root 用户的不一样。

    Root用的是         /bin/bash
    新增用户用的是      /bin/sh

用 `ls -l /bin/sh` 显示：

`/bin/sh -> /bin/dash`

dash 与 bash 是不一样的。

### 处理：

把 /bin/sh 改成 /bin/bash，

命令：

`vi /etc/passwd`

查看用户名后的 bash 种类，修改为 /bin/bash。

注销系统后生效。问题解决。