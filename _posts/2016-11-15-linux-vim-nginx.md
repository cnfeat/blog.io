---
layout: post
title: vim 配置 nginx 配置语法高亮技巧
date: 2016-11-15
categories: blog
tags: [linux,nginx]
description: vim 配置 nginx 配置语法高亮技巧

---

### 首先下载nginx.vim语法文件:

`wget http://www.vim.org/scripts/download_script.php?src_id=14376`

最新版下载地址，可以在此查看：

[http://www.vim.org/scripts/script.php?script_id=1886](http://www.vim.org/scripts/script.php?script_id=1886)

### 然后将其命名为nginx.vim

`mv downloadXXX nginx.vim`

### 移动到 ~/.vim/syntax/

如没这个目录，则先新建。命令如下：
 
    mkdir ~/.vim
    mkdir ~/.vim/syntax
    mv nginx.vim ~/.vim/syntax/
 
### 修改~/.vim/filetype.vim

`vi ~/.vim/filetype.vim`

在新行中加入以下内容

`au BufRead,BufNewFile */nginx*/conf/* set ft=nginx`
 
你可以根据你自己的命名规则来设置这个路径。