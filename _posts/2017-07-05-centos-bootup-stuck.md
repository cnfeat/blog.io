---
layout: post
title: CentOS开机卡在进度条的解决方法
date: 2017-07-05
categories: blog
tags: [centos]
description: CentOS开机卡在进度条的解决方法
---

CentOS开机的时候卡在进度条一直进不去

![](https://azraelgreen.github.io/img/20170705_01.jpg)

这看不出开机启动卡在哪里，只好重启按住"e"键，进入启动菜单：

![](https://azraelgreen.github.io/img/20170705_02.jpg)

接着按e进入编辑第一项：

![](https://azraelgreen.github.io/img/20170705_03.jpg)

然后移动到第二项kernel...接着按e进入编辑

![](https://azraelgreen.github.io/img/20170705_04.jpg)

去掉rhgb quiet字样

![](https://azraelgreen.github.io/img/20170705_05.jpg)

按回车保存回到选择项

![](https://azraelgreen.github.io/img/20170705_06.jpg)

按b启动它就能看到启动过程了

![](https://azraelgreen.github.io/img/20170705_07.jpg)

注意查看启动过程中卡在哪里？可以**按f5键进度条/命令行界面方式切换**，确认卡问题后处理就好。

比如我的就**卡在开机启动MySQL上**，一直进不去系统，所以可以**使用单用户模式进入系统把MySQL启动项关闭后**，再进系统就没有问题了。
