---
layout: post
title: Linux下使用Rdesktop远程访问Windows系统
date: 2017-05-17
categories: blog
tags: [linux]
description: Linux下使用Rdesktop远程访问Windows系统
---

> rdesktop 是 UNIX 和 Linux 系统的一个远程桌面连接软件，它通过 MicrosoftWindows NT、Windows 2000 提供的终端服务(Terminal Services)以及WindowsXP 的远程桌面服务(Remote Desktop)，能在Linux系统下远程登录 Windows 的窗口系统并使用。

### 一、 rdesktop 安装

rpm 安装：

	rpm -ivh rdesktop-1.3.1-3.i386.rpm  
 
编译安装如下：

	tar xvzfrdesktop-1.3.1.tar.gz       
	cdrdesktop-1.3.1       
	./configure       
	make       
	makeinstall 

CentOS 系统在线安装：

	[root@server ~]# yum -y install rdesktop 

Ubuntu 系统安装：

	[root@server ~]$ sudo apt install rdesktop

安装成功后，在 `/usr/local/bin` 下生成了可执行的 `rdesktop` 程序。

### 二、rdesktop 的使用

### 2.1、远程 Windows 系统的设置        

这里以连接Windows XP Professional的远程桌面服务为例。

首先在WindowsXP 下启用远程桌面服务(注意，XP 的HomeEdition 没有远程桌面服务)，

右键点击“我的电脑”，选择“属性”，查看“远程”选项，

选择“`允许用户远程连接到这台计算机`”即可。
         
### 2.2、Linux 下 rdesktop 的使用    
   
rdesktop 的使用很简单，可通过 `#rdesktop -h` 得到使用的帮助。

一般常用的登录命令为：     
  
`#rdesktop -g 1024x768 -a 24 hostname`

	“-g 1024×768”	设置分辨率为1024×768，
	“-a 24”	设置真彩24 位，
	hostname	为 Windows 机器的主机名或者IP 地址。

在输入了Windows XP的用户名和密码后，就可以登录并操作远程的Windows系统。

#### 连接到Windows远程桌面

	[root@server ~]# rdesktop -a 16 192.168.1.96:33389 -u administrator 

PS：此处windows远程桌面端口已经改成 `33389` 端口。

### 三、rdesktop命令使用示例

### 3.1、示例1

	rdesktop -u yourname -p password -g 1024*720 192.168.0.2

rdesktop为使用远程桌面连接的命令，参数说明如下：

	-u 用户名，yourname处为目标客户端的用户名；
	-p ：客户端用户的密码；
	-f : 默认全屏，需要用 Ctrl-Alt-Enter 组合键进行全屏模式切换。
	-g ：分辨率，中间用“x”连接，可省略，省略后默认为全屏显示；
	192.168.0.1 ：目标客户端的IP地址
	-d ：域名，列如域INC 那么参数就是 -d inc
	-r ：多媒体重新定向 比如开启声音 -r sound 使用本地的声卡 -r sound : local 开启u盘：-r disk:usb=/mnt/usbdevice

除了这些常用的选项，rdesktop也支持cdrom, floppy软盘的远程映射，详细可以参考rdesktop命令帮助。

	./rdesktop -h

### 3.2、示例2

	rdesktop 192.168.101.201:8282 -r sound:local -r disk:home=/home -r clipboard:CLIPBOARD  -utest -p123456

参数说明：

	192.168.101.201:8282 ：目标客户端的IP地址和端口号；
	-r sound:local ：重新定向，只用本地的声卡；
	-r disk:home=/home ：把/home文件夹挂载到远程主机上；
	-r clipboard:CLIPBOARD ：允许在远程主机和本机之间共享剪切板，可以复制黏贴；
	-utest ：远程主机的用户名为test；
	-p123456 ：远程主机的密码123456。