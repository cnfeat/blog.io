---
layout: post
title: Instruments和调试器
date: 2016-02-07
categories: blog
tags: [写作,千字文]
description: Instruments和调试器

---

注：此文是2016年02月07日创建。

# Instruments和调试器

## Instruments

#### 1. Instruments用途: 

问题:  一个引用程序的设计和实现功能只是一部分工作, 性能是许多开发者忽略的一个特性. 不注意性能会对代码产生一些更具体的影响. 例如: 一个不注意内存使用的应用程序最终可能会在iOS上呗系统强制退出; 一个消耗大量CPU时间的App可能会耗尽用户的电池电量, 使系统过热, 还有一些资源也是应用程序需要多加留意的, 比如网络宽带和磁盘空间.


为了帮助检测应用程序的性能和资源的使用, 开发者工具中包含了Instruments. 这个应用程序能够检测并报告其他应用程序的各个方面.

#### 2. Instruments工作原理
Instruments 的工作方式是想正在运行的应用程序插入诊断钩. 这些钩子可以做的工作包括: <strong> 分析应用程序的内存使用, 监视App在各种方法上花费的时间, 检查其他数据, 随后将手机, 分析并呈现这些信息</strong>让我们了解应用程序内部的情况.

补充:  这里我会介绍如何使用Instruments, 如何分析应用程序, 如何使用Instruments提供的星系来发现和解决内存问题和性能问题.还会学习如何使用Xcode的调试器来跟踪并解决问题.

#### 3. 使用Instruments

1. 打开一个程序
2. Command-I 打开Instruments
3. 选择其中一个工具(Instrument)
	1. Allocations工具 -> 用于测量内存性能
	2. Time Profiler工具 -> 用于测量运行时性能
	3. VM Tracker工具 -> 用于测量内存占用
	4. 等待
4. 














































