---
layout: post
title: 使用scp在windows和Linux之间互传文件
date: 2017-03-14
categories: blog
tags: [ssh]
description: 使用scp在windows和Linux之间互传文件
---

> 为了进行系统维护操作，有时需要再windows和linux或Unix系统之间互传文件，虽然有很多工具可以实现该功能，但我还是觉得命令行来的方便快捷，起初使用linux的scp命令，总是不成功，网上也没有相关介绍，经过几次努力之后，终于成功的摸索出了scp命令在写windows的路径时的写法，与大家分享：

### 从linux系统复制文件到windows系统：
`scp /home/a.txt  administrator@192.168.0.181:/d:/`

### 在linux环境下，将windows下的文件复制到linux系统中：
`scp administrator@192.168.0.181:/d:/test/config.txt  /home`

### 注意：

因为windows系统本身不支持ssh协议，所以，要想上面的命令成功执行，**必须在windows系统上，安装ssh for windows的客户端软件**，比如winsshd，使windows系统支持ssh协议，并确保win系统上开启了ssh服务。

winsshd官方下载：[https://www.bitvise.com/winsshd-download](https://www.bitvise.com/winsshd-download)