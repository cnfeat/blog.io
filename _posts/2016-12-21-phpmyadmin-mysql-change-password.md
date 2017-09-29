---
layout: post
title: 利用 phpmyadmin 修改 mysql 的 root 密码
date: 2016-12-21
categories: blog
tags: [mysql]
description: 利用 phpmyadmin 修改 mysql 的 root 密码
---

首先用 root 账号登陆 phpmyadmin，

然后点击左侧进入 mysql 数据库，

在顶部点击“mysql”进入sql输入界面。

输入以下命令：

`update user set password=password('123456') where user='root'`

其中 123456 为你希望修改的密码，切记不要在数据库中直接手工修改密码。
 
然后点击右下角的“执行”。

接着还要进入phpmyadmin目录，修改config.inc.php文件。

找到 `$cfg['Servers'][$i]['password']  = ' '`

修改为 `$cfg['Servers'][$i]['password']      = '123456'`

123456就是您想要的密码。

重启mysql后新密码生效。