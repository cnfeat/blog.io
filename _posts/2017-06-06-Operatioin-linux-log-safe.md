---
layout: post
title: linux查看ssh登录日志跟安全措施防护
date: 2017-06-06
categories: blog
tags: [operations,linux]
description: linux查看ssh登录日志跟安全措施防护
---

## linux 查看日志：

linux 下登录日志在下面的目录里：

	cd /var/log

查看 ssh 用户的登录日志：

	less secure

查看 系统日志：

	less messages

最近登录的日志：

	last

## linux日志管理：

### 1.日志简介

日志对于安全来说，非常重要，他记录了系统每天发生的各种各样的事情，你可以通过他来检查错误发生的原因，或者受到攻击时攻击者留下的痕迹。

日志主要的功能有：审计和监测。他还可以实时的监测系统状态，监测和追踪侵入者等等。

在Linux系统中，有三个主要的日志子系统：

#### 连接时间日志：

由多个程序执行，把纪录写入到 `/var/log/wtmp` 和 `/var/run/utmp`，login 等程序更新 wtmp 和 utmp 文件，使系统管理员能够跟踪谁在何时登录到系统。

#### 进程统计：

由系统内核执行。当一个进程终止时，为每个进程往进程统计文件( pacct 或 acct )中写一个纪录。进程统计的目的是为系统中的基本服务提供命令使用统计。

#### 错误日志：

由 syslogd(8) 执行。各种系统守护进程、用户程序和内核通过 syslog(3) 向文件 `/var/log/messages` 报告值得注意的事件。

另外有许多 UNIX 程序创建日志。

像 HTTP 和 FTP 这样提供网络服务的服务器也保持详细的日志。 

#### 常用的日志文件如下：

	access-log 		记录HTTP/web的传输
	acct/pacct 		记录用户命令
	aculog 			记录MODEM的活动
	btmp 			记录失败的纪录
	lastlog 		记录最近几次成功登录的事件和最后一次不成功的登录
	messages 		从syslog中记录信息(有的链接到syslog文件)
	sudolog 		记录使用sudo发出的命令
	sulog 			记录使用su命令的使用
	syslog 			从syslog中记录信息(通常链接到messages文件)
	utmp 			记录当前登录的每个用户
	wtmp 			一个用户每次登录进入和退出时间的永久纪录
	xferlog 		记录FTP会话

utmp、 wtmp 和 lastlog 日志文件是多数重用 UNIX 日志子系统的关键：保持用户登录进入和退出的纪录。

有关当前登录用户的信息记录在文件 `utmp` 中；登录进入和退出纪录在文件 `wtmp` 中；最后一次登录文件可以用 `lastlog` 命令察看。数据交换、关机和重起也记录在 `wtmp` 文件中。所有的纪录都包含时间戳。

这些文件( lastlog 通常不大)在具有大量用户的系统中增长十分迅速。例如 wtmp 文件可以无限增长，除非定期截取。

许多系统以一天或者一周为单位把 wtmp 配置成循环使用。它通常由 cron 运行的脚本来修改。这些脚本重新命名并循环使用 wtmp 文件。通常，wtmp在第一天结束后命名为 wtmp.1;第二天后 wtmp.1 变为 wtmp.2 等等，直到 wtmp. 7。

每次有一个用户登录时，login 程序在文件 lastlog 中察看用户的 UID。如果找到了，则把用户上次登录、退出时间和主机名写到标准输出中，然后 login 程序在 lastlog 中记录新的登录时间。

在新的 lastlog 纪录写入后，utmp 文件打开并插入用户的 utmp 纪录。该纪录一直用到用户登录退出时删除。

utmp 文件被各种命令文件使用，包括 `who`、`w`、`users` 和 `finger` 。

下一步，login 程序打开文件 wtmp 附加用户的 utmp 纪录。当用户登录退出时，具有更新时间戳的同一 utmp 纪录附加到文件中。

wtmp 文件被程序 `last` 和 `ac`使用。

### 2.具体命令

wtmp 和 utmp 文件都是二进制文件，他们不能被诸如 `tail` 命令剪贴或合并(使用`cat`命令)。用户需要使用 `who`、`w`、`users`、`last` 和 `ac` 来使用这两个文件包含的信息。

#### who：

who 命令查询 utmp 文件并报告当前登录的每个用户。

Who 的缺省输出包括用户名、终端类型、登录日期及远程主机。

如果指明了 wtmp 文件名，则 who 命令查询所有以前的纪录。

	who /var/log/wtmp

以上命令，报告自从 wtmp 文件创建或删改以来的每一次登录。

### w：

w 命令查询 utmp 文件并显示当前系统中每个用户和它所运行的进程信息。

### users：

users 用单独的一行打印出当前登录的用户，每个显示的用户名对应一个登录会话。

如果一个用户有不止一个登录会话，那他的用户名把显示相同的次数。

### last：

last 命令往回搜索 wtmp 来显示自从文件第一次创建以来登录过的用户。
 