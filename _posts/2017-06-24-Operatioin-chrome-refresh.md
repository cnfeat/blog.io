---
layout: post
title: Chrome浏览器切换到之前打开的标签页会重新加载问题
date: 2017-06-24
categories: blog
tags: [operations]
description: Chrome浏览器切换到之前打开的标签页会重新加载问题
---

### 问题：

打开一个标签页之后，若切换到其他标签页，再浏览一段比较长的时间（也可能继续打开新的标签页浏览），最后再切换回去的时候就会自动刷新。

### 解决办法：

在地址栏输入

	chrome://flags/#automatic-tab-discarding

设置为 `停用` 即可。

貌似新版本的Chrome默认会**自动舍弃标签页**，用于节省系统内存。
