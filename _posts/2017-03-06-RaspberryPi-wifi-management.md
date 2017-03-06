---
layout: post
title: 树莓派wifi命令行管理
date: 2017-03-06
categories: blog
tags: [RaspberryPi]
description: 树莓派wifi命令行管理
---

查看无线设备：

`iw dev`

检查无线设备情况（假设无线网卡是wlan0）：

`iw dev wlan0 link`

启用无线设备：

`sudo ip link set wlan0 up`

禁用无线设备：

`sudo ip link set wlan0 down`

连接wifi（根据essid连接）：

`sudo iw wlan0 connect [essid]`

通过DHCP获取IP地址：

`sudo dhclient wlan0`