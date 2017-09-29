---
layout: post
title: phpmyadmin修改左侧导航面板数据表列表分页数
date: 2017-06-10
categories: blog
tags: [mysql]
description: phpmyadmin修改左侧导航面板数据表列表分页数
---

### 前言

有时候我们登录 `phpmyadmin` 查找数据表时，如果某个数据库中有几百个表，那么你会发现左侧导航面板里显示有十几页的分页，但每个分页只显示 50 个表。这让我们想凭知觉找到某个字母开头的表，变得不现实，要翻好几页！

那么有什么办法可以让这每个分页显示更多的表呢？经过一翻网上搜索，终于找到了以下办法。

### 方法一：仅对本次登录有效方法：

1、网页登录 `phpmyadmin`

2、依次点击：`设置` - `导航面板`

3、修改 `节点中最大项数`，默认为 `50`，可以修改为 `300`

![](https://azraelgreen.github.io/img/20170610_phpmyadmin.png)

4、点击页面下的 `应用`

### 方法二：永久有效方法（修改配置文件config.inc.php）：

1、通过 `ssh` 客户端连接到服务器，进入到 `phpmyadmin` 目录

2、先备份配置文件

`cp config.inc.php config.inc.php.bak`

3、修改配置文件

`vi config.inc.php`

添加项目：

	$cfg['MaxNavigationItems'] = 300;

保存退出。
