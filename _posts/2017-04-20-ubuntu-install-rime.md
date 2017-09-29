---
layout: post
title: Ubuntu系统下安装fcitx-rime，并添加五笔/五笔拼音
date: 2017-04-20
categories: blog
tags: [ubuntu]
description: Ubuntu系统下安装fcitx-rime，并添加五笔/五笔拼音
---

### 简介

Rime，全名：中州韵输入法引擎，它不仅仅是一个输入法，也是一个输入法算法框架。Rime 支持拼音、双拼、注音、五笔和仓颉等音码、形码输入法。

官网：[http://rime.im/](http://rime.im/)

### 安装

Ubuntu/Debian 用户可以使用下面命令安装：

	sudo apt-get install fcitx-rime 	#Fcitx 用户
	sudo apt-get install ibus-rime 		#IBus 用户 （ibus不作介绍，因为很繁琐）

默认情况下安装的fcitx-rime中是没有五笔的。添加方法如下：

	sudo apt-get install librime-data-wubi
	cp /usr/share/rime-data/wubi86.schema.yaml ~/.config/fcitx/rime/
	cp /usr/share/rime-data/wubi_pinyin.schema.yaml ~/.config/fcitx/rime/
	ln -s /usr/share/rime-data/wubi86.dict.yaml ~/.config/fcitx/rime/
	ln -s /usr/share/rime-data/wubi_pinyin.prism.bin ~/.config/fcitx/rime/
	ln -s /usr/share/rime-data/pinyin_simp.dict.yaml ~/.config/fcitx/rime/
	ln -s /usr/share/rime-data/pinyin_simp.* ~/.config/fcitx/rime/
	ln -s /usr/share/rime-data/wubi86.* ~/.config/fcitx/rime/

编辑 `~/.config/fcitx/rime/default.yaml`，在 `schema_list` 下方加一行：

	- schema: wubi_pinyin
	- schema: wubi86


重启 fcitx 后，按 `F4` 选择五笔/五笔拼音，配置完毕。

更多信息请参考官方文档：[https://github.com/rime/home/wiki/RimeWithIBus](https://github.com/rime/home/wiki/RimeWithIBus)