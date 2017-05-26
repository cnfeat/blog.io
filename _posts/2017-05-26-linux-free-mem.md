---
layout: post
title: linux系统内存使用情况解读
date: 2017-05-26
categories: blog
tags: [linux]
description: linux系统内存使用情况解读
---

### 查看Linux服务器的内存使用情况

可以使用命令

	free -m

注意此命令只在Linux下有效，在FreeBSD中没有此命令。
 
	used：已经使用的内存数

	free：空闲的内存数

	shared：多个进程共享的内存总额

	-buffers/cache：(已用)的内存数，即 used-buffers-cached

	+buffers/cache：(可用)的内存数，即 free+buffers+cached

得出结论：

### 可用内存的计算公式为：

	可用内存 = free + buffers + cached

更形象地表示上面的关系：

![](https://azraelgreen.github.io/img/20170526_linux_free_mem.png)

Linux管理内存的机制非常优秀，简言之：Linux的内存是拿来用的，而不是拿来看的。

我与一个朋友探讨Linux的使用情况时，他问我为什么Linux使用的内存这么高。他机器上1GB的内存free才232MB，而Windows XP才用了200MB不到的样子。这其实是被Linux的free命令之表象迷惑了，Linux的内存使用是很有讲究的。

**-buffers/cache 反映的是被程序实实在在用掉的内存，而 +buffers/cache 反映的是可以挪用的内存总数。**

第一部分( **Mem-此部分是从OS层面来看的** )与第二部分(-/+buffers/cache)的结果有关，used和free为什么这么奇怪？其实我们可以从两个方面来分析。

对操作系统来讲这两项是Mem的参数，buffers/cached都属于被使用，所以它认为free只有232MB；

第二部分内容( **-/+buffers/cache-此部分是从应用层面来看的** )对应用程序来讲+buffers/cached等同于可用的内存，因为buffer/cached可提高程序执行的性能，当程序使用内存时，buffer/cached很快就会被使用。

所以从应用的角度来看，应以(-/+buffers/cache)的free和used为主，即我们主要看与它相关的free和used就可以了。

另外告诉大家一些常识，为了提高磁盘和内存的存取效率，对Linux做了很多精心的设计，除了对dentry进行缓存(用于VFS、加速文件路径名到inode的转换)外，还采取了两种主要Cache方式：**Buffer Cache和Page Cache**。

**前者用于针对磁盘块的读写，后者用于针对文件inode的读写。**这些Cache能有效地缩短I/O系统调用(比如read、write、getdents)的时间。

在Linux中，内存是拿来用的，不是拿来看的。而在Windows中，无论你的真实物理内存有多少，它都会用硬盘交换文件来读，即使是内存还有一大部分。这也就是Windows常常提示虚拟空间不足的原因。可以想见，硬盘怎么会快过内存。

所以我们**在观察Linux的内存使用情况时，只要没发现用swap的交换空间，就不用担心自己的内存太少。**如果常常看到swap用了很多，那么你就要考虑加物理内存了。这也是在Linux服务器上看内存是否够用的标准。

我们通过free命令查看机器空闲内存时，会发现free的值很小。这主要是因为，在linux中有这么一种思想，内存不用白不用，因此它尽可能的cache和buffer一些数据，以方便下次使用。但实际上这些内存也是可以立刻拿来使用的。