---
layout: post
title: vim 语法高亮显示设置
date: 2016-11-08
categories: blog
tags: [linux]
description: vim 语法高亮显示设置

---

在终端下使用vim进行编辑时，默认情况下，编辑的界面上是没有显示行号、语法高亮度显示、智能缩进等功能的。

为了更好的在vim下进行工作，需要手动设置一个配置文件：**.vimrc**

### 让vi“出彩”的方法：

`vi ~/.bashrc`

添加如下语句：

`alias vi='vim'`

*注：配置vi时，要编辑的文件是用户主目录下的~/.vimrc文件，而不是/etc/vimrc文件。如何编辑了/etc/vimrc，改变的是所有用户的vi配置。*

`vi ~/.vimrc`

添加如下内容：

```
syntax on
set showmode
set autowrite
set number
set autoindent
set showmatch
set tabstop=4    //tab设置为4个空格
```

注：vim 使用中暂时取消显示行号：`:set nonu`

.vimrc 中使用注释，要 " 双引号开始