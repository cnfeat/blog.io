---
layout: post
title: windows系统下安装php的redis插件方法
date: 2017-09-07
categories: blog
tags: [php]
description: windows系统下安装php的redis插件方法
---

## 第一步，windows下安装和配置Redis

### 一、下载windows版本的Redis

官网上不提供windows版本的，现在官网没有下载地址，只能在github上下载，官网只提供linux版本的下载

官网下载地址：[redis.io/download](http://redis.io/download)

github下载地址：[github.com/MSOpenTech/redis/tags](http://github.com/MSOpenTech/redis/tags)

这里我选择的是x64-3.2.100，下载的时候下载msi(不要下载zip的压缩包)


### 二、安装Redis

设置Redis的服务端口，默认为 `6379`，默认就好，单击next

选择安装的路径，并且打上勾（这个非常重要），`添加到path` 是把Redis设置成windows下的服务，不然你每次都要在该目录下启动命令 `redis-server redis.windows.conf`，但是只要一关闭cmd窗口，redis就会消失，这样就比较麻烦。

设置 `Max Memory`，然后next进入安装

如果redis的应用场景是作为db使用，那不要设置这个选项，因为db是不能容忍丢失数据的。
如果作为cache缓存那就得看自己的需要（我这里设置了 `1024M` 的最大内存限制）

指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区。


### 三、测试所安装的Redis如果你是和我一样通过msi文件的安装，你可以在计算机管理→服务与应用程序→服务  看到Redis正在运行

你也可以将它停止，(不停止会出现错误代码为18012的错误，表示本机端口6379被占用)
然后在cmd窗口进入Redis的安装路径的根目录 
 
输入命令

	redis-server.exe redis.windows.conf

启动Redis服务

下面进行测试：

你可以在Redis的安装根目录下找到 `redis-cli.exe` 文件启动(我用的是这种方法)

或在cmd中先进入Redis的安装根目录用命令 `redis-cli.exe -h 192.168.10.61 -p 6379` （注意换成自己的IP）的方式打开

测试方法：设置键值对    取出键值对

### 四、测试成功，安装完成

## 第二步，安装php的redis插件


### 1.查看php版本信息

使用php测试网页：

	<?php
		echo phpinfo();
	?>

如我的php版本是5.6.27

### 2.根据PHP版本号，编译器版本号和CPU架构去下载

下载地址：[https://pecl.php.net/package/redis/2.2.7/windows](https://pecl.php.net/package/redis/2.2.7/windows)

选择对应的版本（VC的版本，64位还是32位）：`php_redis-2.2.7-5.6-ts-vc11-x64.zip`

### 3.解压缩后，将 `php_redis.dll` 和 `php_redis.pdb` 拷贝至 `php` 的 `ext` 目录

### 4.修改php.ini

（如果装的是wamp的话）配置文件位置：

	D:\wamp\bin\apache\apache2.4.23\bin\php.ini

添加如下内容：

	; php_redis
	extension=php_igbinary.dll
	extension=php_redis.dll

### 5.重启Apache后，使用phpinfo查看扩展是否成功安装