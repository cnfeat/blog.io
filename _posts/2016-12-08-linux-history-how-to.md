---
layout: post
title: History（历史）命令用法 15 例
date: 2016-12-08
categories: blog
tags: [linux]
description: History（历史）命令用法 15 例
---

如果你经常使用 Linux 命令行，那么使用 history（历史）命令可以有效地提升你的效率。本文将通过实例的方式向你介绍 history 命令的 15 个用法。

## 1、使用 HISTTIMEFORMAT 显示时间戳

当你从命令行执行 history 命令后，通常只会显示已执行命令的序号和命令本身。如果你想要查看命令历史的时间戳，那么可以执行：

    # export HISTTIMEFORMAT='%F %T '
    # history | more
    1  2008-08-05 19:02:39 service network restart
    2  2008-08-05 19:02:39 exit

注意：这个功能只能用在当 HISTTIMEFORMAT 这个环境变量被设置之后，之后的那些新执行的 bash 命令才会被打上正确的时间戳。在此之前的所有命令，都将会显示成设置 HISTTIMEFORMAT 变量的时间。

[注：你也可以设置 alias 语句来查看最近的历史命令]

    alias h1='history 10' 
    alias h2='history 20' 
    alias h3='history 30' 

## 2、使用 Ctrl+R 搜索历史

Ctrl+R 是我经常使用的一个快捷键。此快捷键让你对命令历史进行搜索，对于想要重复执行某个命令的时候非常有用。

当找到命令后，通常再按回车键就可以执行该命令。

如果想对找到的命令进行调整后再执行，则可以按一下**左或右方向键**。

## 3、快速重复执行上一条命令

有 4 种方法可以重复执行上一条命令：

1. 使用上方向键，并回车执行。
2. 按 !! 并回车执行。
3. 输入 !-1 并回车执行。
4. 按 Ctrl+P 并回车执行。

## 4、从命令历史中执行一个指定的命令

在下面的例子中，如果你想重复执行第 4 条命令，那么可以执行 !4：

    # history | more
    1  service network restart
    2  exit
    3  id
    4  cat /etc/redhat-release
    # !4
    cat /etc/redhat-release

## 5、通过指定关键字来执行以前的命令

在下面的例子，输入 !ps 并回车，将执行以 ps 打头的命令：

    # !ps
    ps aux | grep yp

## 6、使用 HISTSIZE 控制历史命令记录的总行数

将下面两行内容追加到 **.bash_profile** 文件并重新登录 bash shell，命令历史的记录数将变成 450 条：

    # vi ~/.bash_profile
    HISTSIZE=450
    HISTFILESIZE=450

## 7、使用 HISTFILE 更改历史文件名称

默认情况下，命令历史存储在 **~/.bashhistory** 文件中。添加下列内容到 .bashprofile 文件并重新登录 bash shell，将使用 .commandline_warrior 来存储命令历史：

    # vi ~/.bash_profile
    HISTFILE=/root/.commandline_warrior

## 8、使用 HISTCONTROL 从命令历史中剔除连续重复的条目(重复的条目只显示一次)

命令被连续执行了三次。执行 history 后你会看到三条重复的条目。要剔除这些重复的条目，你可以**将 HISTCONTROL 设置为 ignoredups**：

    # export HISTCONTROL=ignoredups

## 9、使用 HISTCONTROL 清除整个命令历史中的重复条目

上例中的 ignoredups 只能剔除连续的重复条目。要清除整个命令历史中的重复条目，可以将 HISTCONTROL 设置成 erasedups：

    # export HISTCONTROL=erasedups

## 10、使用 HISTCONTROL 强制 history 不记住特定的命令

将 HISTCONTROL 设置为 **ignorespace**，并在不想被记住的命令前面输入一个空格：

    # export HISTCONTROL=ignorespace
    # ls -ltr
    # pwd
    #  service httpd stop [这条会被忽略记录]

## 11、使用 -c 选项清除所有的命令历史

如果你想清除所有的命令历史，可以执行：

    # history -c

## 12、命令替换

在下面的例子里，!!:$ 将为当前的命令获得上一条命令的参数：

    # ls anaconda-ks.cfg
    anaconda-ks.cfg
    # vi !!:$
    vi anaconda-ks.cfg

补充：**使用 !$ 可以达到同样的效果**，而且更简单。

下例中，**!^ 从上一条命令获得第一项参数**：

    # cp file1 file1.bak
    # vi !^
    vi file1

## 13、为特定的命令替换指定的参数

在下面的例子，!cp:2 从命令历史中搜索以 cp 开头的命令，并**获取它的第二项参数**：

    # cp ~/longname.txt /really/a/very/long/path/long-filename.txt
    # ls -l !cp:2
    ls -l /really/a/very/long/path/long-filename.txt

下例里，!cp:$ 获取 cp 命令的**最后一项参数**：

    # ls -l !cp:$
    ls -l /really/a/very/long/path/long-filename.txt 

## 14、使用 HISTSIZE 禁用 history

如果你想禁用 history，可以将 HISTSIZE 设置为 0：

    # export HISTSIZE=0

## 15、使用 HISTIGNORE 忽略历史中的特定命令

下面的例子，将忽略 pwd、ls、ls -ltr 等命令(命令之间中用:分隔)：

    # export HISTIGNORE=”pwd:ls:ls -ltr:”
    # pwd
    # ls
    # ls -ltr
    # service httpd stop
    # history | tail -3
    79  export HISTIGNORE=”pwd:ls:ls -ltr:”
    80  service httpd stop
    81  history