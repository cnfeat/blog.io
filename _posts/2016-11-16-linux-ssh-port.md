---
layout: post
title: Linux 下 SSH 端口修改
date: 2016-11-16
categories: blog
tags: [linux,ssh]
description: Linux 下 SSH 端口修改

---

Linux各发行版中SSH端口默认为22，如果正式做站或其它用途，为了提高安全性就需要修改掉默认的SSH端口号，防止被有心人穷举密码。

### 1、修改配置文件：/etc/ssh/sshd_config ，找到#port 22

	vi /etc/ssh/sshd_config

### 2、先将Port 22 前面的 # 号去掉，并另起一行。

如定义SSH端口号为26611 ，则输入

	Port 26611

自定义端口选择建议在万位的端口（如：10000-65535之间）

可以先同时开两个端口：

	port 22
	port 26611

手动指定SSH端口为22和26611（双端口号），先暂保留22端口，防止防火墙屏蔽了其它端口导致无法连接要给自己留条“后路”。


### 3、修改完毕后，重启SSH服务，并退出当前连接的SSH端口。

	service sshd restart

### 4、重启完毕，尝试使用新端口登陆

	ssh -p 26611 test@192.168.xx.xx

连接成功，需要重新添加SSH-RSA验证，选择是（或Yes）即可。

### 5、若能正常访问，返回第一步的配置文件，将原port 22这一行删掉，再按第三步重启SSH即可。

以上步骤重启后使用默认22号端口无法进入SSH，达到目的。
