---
layout: post
title: grep 在文本中查找内容
date: 2016-11-22
categories: blog
tags: [linux]
description: grep 在文本中查找内容

---

### 功能：

grep系列是Linux中使用频率最高的文本查找命令。主要功能在一个或者多个文件中查找特定模式的字符串。如果该行有匹配的字符串，则输出整个行的内容。如果没有匹配的内容，则不输出任何内容。grep命令不改动源文件。

Linux的grep家族包括grep、egrep、fgrep、rgrep。grep可以通过-G、-E、-F命令行选项来使用egrep和fgrep的功能。

### 语法：

`grep    [选项]   PATTERN  [FILE]`

在每个 FILE 或是标准输入中查找 PATTERN。

默认的 PATTERN 是一个基本正则表达式(缩写为 BRE)。

### 例如: 

`grep -i 'hello world' menu.h main.c`

`cat test.txt | grep -B2 "hello" | less`

`grep -v root /etc/passwd | grep -v nologin `

将/etc/passwd，没有出现 root 和 nologin 的行取出来

### 常用参数：

    -i   --ignore-case	         忽略大小写
    -n   --line-number           显示符合样式的行前，标出该行的列数
    -f   --file=FILE             从 FILE 中取得 PATTERN
    -c   --count                 仅仅打印每个文件的匹配次数
    -v   --invert-match          打印不匹配的行

    -B   --before-context=NUM    显示匹配行和之前的几行
    -A   --after-context=NUM     显示匹配行和之后的几行
    -C   --context=NUM           显示匹配行和之前及之后的几行