---
layout: post
title: Archlinux 的 pacman 常用命令
date: 2016-10-14
categories: blog
tags: [Archlinux]
description:  Archlinux 的 pacman 常用命令

---

Arch的包管理器 pacman 还是比较好用的，

### 一、常用的 pacman 命令：

#### 系统更新：
`pacman -Syu`

#### 安装软件：
`pacman -S 软件包名`

#### 查询软件：
`pacman -Ss 软件包名`

#### 卸载软件：
`pacman -R 软件包名`

#### 系统清理：
`pacman -Scc && pacman -Sc`

### 二、常用其他命令：

#### 查看IP：
`ip addr`

#### 显示网卡名字：
`ip link`