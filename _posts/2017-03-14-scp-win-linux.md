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

以上方法是依赖于远端win系统上架设ssh服务实现的。

---

那么相反地，如果你是在直接在win系统上操作，就不需要在win系统架设ssh服务，只要有支持scp命令的客户端软件就可以。比如说git的win客户端命令行下。

### 在win系统上，传输本地文件到远端linux系统命令：

`scp /d/test.txt root@192.168.0.243:/root`

### 拷贝远端linux系统文件到本地win系统的D盘上：

`scp root@192.168.0.243:/root/test1.txt /d`

git for win 官方下载：[https://git-scm.com/download/win](https://git-scm.com/download/win)

---

个人平时办公电脑是win系统，安装了xshell作为ssh客户端，同时还安装了xftp用于文件传输。只要通过xshell连接到了远程的linux服务器，直接点击xshell窗口上方的xftp快捷图标，弹出类似于其他ftp软件的窗口，直接上传或下载就可以了，非常方便！