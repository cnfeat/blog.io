---
layout: post
title: php-fpm.conf设置详解
date: 2017-02-21
categories: blog
tags: [php]
description: php-fpm.conf设置详解
---

### 一、配置文件位置：

LNMP一键安装包下，php-fpm.conf 位置：

`vi /usr/local/php/etc/php-fpm.conf`

yum 安装位置：

`vi /etc/php-fpm.d/www.conf`

### 二、进程管理种类介绍：

对于进程的管理存在两种风格——`static`和`dynamic`。和之前的版本的进程管理其实还是一样的，只是将apache-like改成了dynamic，这样更容易理解。

#### 默认：`pm = dynamic`

如果设置成 `static`，php-fpm进程数自始至终都是`pm.max_children`指定的数量，不再增加或减少。

如果设置成 `dynamic`，则php-fpm进程数是动态的，最开始是`pm.start_servers`指定的数量，如果请求较多，则会自动增加， 保证空闲的进程数不小于`pm.min_spare_servers`，如果进程数较多，也会进行相应清理，保证多余的进程数不多于 `pm.max_spare_servers`。

这两种不同的进程管理方式，可以根据服务器的实际需求来进行调整。

### 三、具体参数介绍：

这里先说一下涉及到这个的几个参数，他们分别是

pm、pm.max_children、pm.start_servers、pm.min_spare_servers和pm.max_spare_servers。

pm表示使用那种方式，有两个值可以选择，就是`static`（静态）或者`dynamic`（动态）。在更老一些的版本中，dynamic被称作apache-like。这个要注意看配置文件的说明。

#### 下面4个参数的意思分别为：

`pm.max_children`：静态方式下开启的php-fpm进程数量。
`pm.start_servers`：动态方式下的起始php-fpm进程数量。
`pm.min_spare_servers`：动态方式下的最小php-fpm进程数量。
`pm.max_spare_servers`：动态方式下的最大php-fpm进程数量。

#### 如果dm设置为`static`

那么其实只有`pm.max_children`这个参数生效。系统会开启设置数量的php-fpm进程。

#### 如果dm设置为 `dynamic`

那么`pm.max_children`参数失效，后面3个参数生效。系统会在 php-fpm 运行开始 的时候启动`pm.start_servers`个 php-fpm 进程，然后根据系统的需求动态在`pm.min_spare_servers`和 `pm.max_spare_servers`之间调整 php-fpm 进程数。

### 四、对于我们的服务器，选择哪种执行方式比较好呢？

事实上，跟Apache一样，运行的PHP程序在执行完成后，或多或少会有内存泄露的问题。这也是为什么开始的时候一个php-fpm进程只占用3M左右内存，运行一段时间后就会上升到20-30M的原因了。

#### 1、对于内存大的服务器（比如8G以上）来说，指定静态的 `max_children` 实际上更为妥当

因为这样不需要进行额外的进程数目控制，会提高效率。因为频繁开关php-fpm进程也会有时滞，所以内存够大的情况下开静态效果会更好。

**数量可以根据 内存/30M 得到**，比如8GB内存可以设置为100，那么php-fpm耗费的内存就能控制在 2G-3G的样子。如果内存稍微小点，比如1G，那么指定静态的进程数量更加有利于服务器的稳定。这样可以保证php-fpm只获取够用的内存，将不多的内存分配给其他应用去使用，会使系统的运行更加畅通。

#### 2、对于小内存的服务器来说，推荐使用动态方式

比如256M内存的VPS，即使按照一个20M的内存量来算，10个php-cgi进程就将耗掉200M内存，那系统的崩溃就应该很正常了。因此应该尽量地控制php-fpm进程的数量，大体明确其他应用占用的内存后，给它指定一个静态的小数量，会让系统更加平稳一些。或者使用动态方式，因为**动态方式会结束掉多余的进程，可以回收释放一些内存，所以推荐在内存较少的服务器或VPS上使用。**

具体最大数量根据 内存/20M 得到。比如说512M的VPS，建议`pm.max_spare_servers`设置为20。至于`pm.min_spare_servers`，则建议根据服务器的负载情况来设置，比较合适的值在5~10之间。