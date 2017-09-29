---
layout: post
title: ubuntu 桌面版开启 SSH 服务
date: 2016-09-23
categories: blog
tags: [ubuntu]
description: ubuntu 桌面版开启 SSH 服务

---

Ubuntu服务器版，安装好后已自动开启SSH服务，不需要另外配置；而桌面版默认是没有安装ssh服务的。

### 如何区分桌面版和服务器版：

服务器版文件名如：ubuntu-16.04-server-i386.iso

桌面版文件名如：ubuntu-16.04-desktop-i386.iso


### 桌面版安装 ssh 服务：

SSH 分客户端 **openssh-client** 和 服务端 **openssh-server**


#### 只是想登陆别的机器的SSH只需要安装openssh-client

ubuntu有默认安装，如果没有则

`sudo apt-get install openssh-client`


#### 要使本机开放SSH服务就需要安装openssh-server

`sudo apt-get install openssh-server`

确认sshserver是否启动：

`ps -e |grep ssh`

如果看到sshd那说明ssh-server已经启动了。


如果没有则可以这样启动：

`sudo /etc/init.d/ssh start` 

或者 

`service ssh start`

### ssh服务的配置

ssh-server 配置文件位于 **/etc/ssh/sshd_config**

在这里可以定义SSH的服务端口

默认端口是22，你可以自己定义成其他端口号，如222


### ssh服务常用命令

然后重启SSH服务：

    sudo /etc/init.d/ssh stop
    sudo /etc/init.d/ssh start

或者

`sudo /etc/init.d/ssh restart`


#### 登陆 SSH 方法：

`ssh username@192.168.1.112` 

username 为 192.168.1.112 机器上的用户，需要输入密码。