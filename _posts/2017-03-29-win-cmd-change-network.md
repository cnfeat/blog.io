---
layout: post
title: 在windows中用 netsh 命令修改ip地址网关和DNS
date: 2017-03-29
categories: blog
tags: [windows]
description: 在windows中用 netsh 命令修改ip地址网关和DNS
---

下面是两个关于netsh的用法，将它们复制到文本文档中，将后缀名 .txt 改为 .cmd直接双击就可以执行：

### 第一个是用 `netsh` 命令来修改电脑的IP地址,子网掩码,默认网关和DNS.

	@echo off
	netsh interface ip set address "本地连接" static 192.168.0.54 255.255.255.0 192.168.0.1 1
	netsh interface ip set dns "本地连接" static 192.168.0.1
	netsh interface ip add dns "本地连接" 8.8.4.4 2

#### 注意:

其中第二,三,四行中 "本地连接" 一般不需要修改，这要看你的电脑中右击 “网上邻居”，选择“属性”出现“网络连接”窗口中的连接名而定，一般的只有一个“本地连接”。

第二行中的`192.168.0.54  255.255.255.0  192.168.0.1` 三个地址依次为IP地址，子网掩码和默认网关，把它们换成你要修改的地址。

第三行中的 `192.168.0.1` 为DNS的地址，把它换成你要修改的DNS地址即可。

第四行中的 `8.8.4.4` 为辅助DNS地址，也就是第二个，把它换成你要修改的第二个DNS地址即可，如果没有的话，可以把第四行直接删除即可。

### 第二个是用 `netsh` 命令来修改电脑的IP地址,子网掩码,默认网关和DNS为动态获取.

	@echo off
	netsh interface ip set address "本地连接" dhcp
	netsh interface ip set dns "本地连接" dhcp

#### 注意：

这个比较简单，只需根据自己电脑的情况，修改一下"本地连接" 即可，一般不需要修改。