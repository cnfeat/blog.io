---
layout: post
title: 树莓派花生壳内网穿透攻略
date: 2017-03-02
categories: blog
tags: [RaspberryPi]
description: 树莓派花生壳内网穿透攻略
---

### 一、下载花生壳安装包：

先给树莓派接上电源和网线，并下载安装包[phddns_raspberry.tgz](http://hsk.oray.com/download/#type=http|shumeipai)（点击链接下载）
 
### 二、下载好安装包Copy到树莓派，在任意路径下解压并安装：

注：以下操作通过SSH远控到树莓派，在Root权限下进行

解压安装包

`tar zxvf phddns_raspberry.tgz`

进入phddns2文件夹

`cd phddns2`

开始安装

`./oraynewph start`

提示`Oraynewph start success`

说明花生壳成功安装运行。
 
### 三、查看运行状态及SN码：

在任意路径命令行执行

`oraynewph status`

即可看到花生壳运行状态以及SN码（复制备用）。

### 四、登录管理后台并设置内网映射

在浏览器输入地址：`b.oray.com`，在花生壳管理页面，输入SN码与密码（首次登录默认密码`admin`）

首次登录需要修改默认密码，完善手机验证和邮箱信息完成初始化。

树莓派跟花生棒不一样，没有内置帐号，需要绑定花生壳账号使用。绑定登录成功，可以直接点击页面中间橙色模块 内网映射 添加映射。

映射添加完成，然后复制外网访问地址，测试访问情况。
 
### 五、花生壳重置，停止服务和卸载

#### 5.1、花生壳重置

如果想不起登录密码，树莓派花生壳其实是有重置功能的，只要执行一条简单的命令就可以了。在任意路径下运行命令 

`oraynewph reset`

重新登录花生壳管理后台，使用初始密码`admin`登录重置。

备注：如果出现reset failed！try again 是因为在管理后台没有绑定账号，所以无须再次重置。
 
#### 5.2、停止服务

在任意路径命令行执行

`oraynewph stop`

即可关闭运行中的花生壳服务。

 
#### 5.3、卸载花生壳：

在任意路径执行

`oraynewph uninstall`

即可卸载花生壳服务。

我们可以通过命令

`oraynewph status`

确认花生壳服务已经成功卸载。
