---
layout: post
title: 重新系统，如何备份vistualbox虚拟主机
date: 2017-03-23
categories: blog
tags: [virtualbox]
description: 重新系统，如何备份vistualbox虚拟主机
---

我非常喜欢使用virtualbox，不过在重装系统或将虚拟系统转移到其他电脑，如何办呢？

### 第一种方法比较快捷，直接备份目录文件：

在win系统上安装，一般默认配置文件目录保存在：`C:\Users\Administrator\.VirtualBox`

1. 找到以上**.VirtualBox**目录，备份到其他分区
2. 再找到 VirtualBox 保存虚拟主机的目录 **VirtualBox VMs**，如果不在C盘就没关系，不用移动也可以。
3. 重装系统
4. 重装 VirtualBox 程序，把**.VirtualBox**拷贝回原来位置
5. 打开 VirtualBox，选择菜单栏上的 **控制——注册**，逐个选择虚拟机目录里的 .vbox文件，就可以在软件里看到原来的虚拟主机了。

### 第二种方法是要利用它自已提供的导入、导出功能来完成：

1. 点击菜单上的 **管理 - 导出**虚拟电脑。
2. 选择要导出的虚拟电脑。
3. 点击 下一步，选择保存位置，格式为 OVF。
4. 再点击 下一步，选择 导出 即可。
5. 重新系统和软件后，再选择 **管理 - 导入**。

不过经过测试，备份速度实在太慢！我最终还是选择使用第一种方法。

