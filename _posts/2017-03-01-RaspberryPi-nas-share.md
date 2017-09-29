---
layout: post
title: 树莓派实现NAS家庭服务器
date: 2017-03-01
categories: blog
tags: [RaspberryPi]
description: 树莓派实现NAS家庭服务器
---

## 安装samba实现文件共享

### 将硬盘挂载到树莓派上

树莓派开机后，用putty连接后，为方便操作直接进入root用户，然后运行`df –h`，查看硬盘挂载情况。

	root@raspberrypi:/home/pi# df -h
	Filesystem              Size        Used        Avail     Use%      Mounted on
	rootfs                  2.9G       2.4G        387M     87%        /
	/dev/root               2.9G       2.4G        387M     87%        /
	devtmpfs                183M          0        183M      0%        /dev
	tmpfs                    38M       792K         37M      3%        /run
	tmpfs                   5.0M          0        5.0M      0%        /run/lock
	tmpfs                    75M          0         75M      0%        /run/shm
	/dev/mmcblk0p1           56M       9.7M         47M     18%        /boot
	/dev/sda1                70G        24M         67G      1%        /media/nas
 
最后一行 `/dev/sda1` 说明硬盘已经挂载。为下一步安装samba，将共享文件夹设为`/samba`。于是新建文件夹:

	mkdir /samba

设置访问权限：

	shmod 777 /samba

将硬盘挂载到 `/samba` 文件夹，具体步骤：

	umount /dev/sda1   #取消挂载
	mount /dev/sda1 /samba

这里再查看`df -h`，是否已挂载成功。

### 解决硬盘的自动挂载

每次树莓派重启或者硬盘插拔都需要对硬盘进行重新挂载，比较麻烦，因此需要自动挂载。这里要修改`/etc/fstab`文件。

`vi /etc/fstab`

fstab文件其实就是一个表格，表格各列的含意如下：

	第1列：磁盘分区名/卷标，一般是/dev/sdaN（N表示正整数）
	第2列：挂载点，我们在这里把/dev/sda1挂到/samba上。
	第3列：缺省设置，一般用defautls。
	第4列：是否备份：0——表示不做 dump 备份；1——表示要将整个 <fie sysytem> 里的内容备份；2 也表示要做 dump 备份，但该分区的重要性比 1 小。
	第5列：检测顺序：0——不进行检测；根分区（/），必须填写 1，其它的都不能填写 1。如果有分区填写大于 1 的话，则在检查完根分区后，从小到大依次检查下去。

### 安装samba

更新一下源：

`sudo apt-get update`

安装samba

`sudo apt-get install samba samba-common-bin`

安装完成后，配置 `/etc/samba/smb.conf` 文件

在其最后添加以下命令：
 
	[share]                                   #共享文件的名称，将在网络上以此名称显示
		path = /samba                     #共享文件的路径
		valid users = root pi             #允许访问的用户，这里我用的是root 和 pi 两个用户
		browseable = yes                  #允许浏览                                 
		public = yes                      #共享开放                                      
		writable = yes                    #可写
 
保存后，重启samba服务，输入

`/etc/init.d/samba restart`

最后添加共享用户：

`smbpasswd –a pi  #这里我用的pi。`

设置开机自启动，编辑`/etc/rc.local`，添加自动运行语句：

`sudo /etc/init.d/samba restart`

### 测试samba安装效果

在windows计算机上，打开我的电脑，在左下角网络点右键，选映射网络驱动器

点击完成会提示输入用户名和密码，这里输入设置的共享用户名和密码。

最后在计算机下会出现共享的文件夹，点开文件夹，新建test.txt文件进行一下测试，如果能正常建立，就说明ok了，如果不行，应该是权限问题，可再重新设置一下`/samba`文件夹权限。

这里注意，如果在`/samba`文件夹下新建新的文件夹，也需要设置权限，可以用vnc连接后，用管理员浏览，点右键设置文件夹权限为read and write，也可以用chmod命令设置。
 