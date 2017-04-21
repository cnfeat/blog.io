---
layout: post
title: ubuntu16.10安装keepass简体中文语言
date: 2017-04-21
categories: blog
tags: [ubuntu]
description: ubuntu16.10安装keepass简体中文语言
---

### Keepass汉化方法：把下载过来的语言文件拷贝到应用程序所在目录

在ubuntu中安装keepass简体中文语言，需要把下载过来的 `Chinese_Simplified.lngx` 拷贝到keepass应用程序所在目录。

在Windows下的路径很好找，但是对于不熟悉ubuntu系统的用户来说可能会找不到keepass的安装目录。

在 ubuntu16.10 版本中 keepass 默认安装路径是：`/usr/lib/keepass2`

### 方法一：通过打开GUI窗口，直接复制粘贴文件

拷贝过程中你可能会需要保存文件的权限，可以使用命令：

	sudo nautilus（以root权限打开一个窗口，来管理文件）

完整命令：

	sudo nautilus /usr/lib/keepass2

在打开的窗口中，将下载过来的语言文件拷贝到这个文件夹中即可。

### 方法二：可以直接用命令完成整个拷贝操作

在命令行下进入到语言文件所在目录，执行以下语句：

	sudo cp Chinese_Simplified.lngx /usr/lib/keepass2/

按照提示，输入当前用户的密码，完成拷贝。

### 备注：

Keepass 官网下载：[http://keepass.info/download.html](http://keepass.info/download.html)

语言文件下载：[http://keepass.info/translations.html](http://keepass.info/translations.html)