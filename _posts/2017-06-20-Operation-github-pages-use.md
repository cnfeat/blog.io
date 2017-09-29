---
layout: post
title: 如何搭建一个独立博客(简明 Github Pages 与 jekyll 教程)
date: 2017-06-20
categories: blog
tags: [operations]
description: 如何搭建一个独立博客(简明 Github Pages 与 jekyll 教程)
---

## 为什么选择GitHub Pages？

很多人用 wordpress，那为什么要用 github pages 来搭建呢？

	* github pages有300M免费空间，资料自己管理，保存可靠；
	* 熟悉 github 工作原理，方便团队协作流程；

## GitHub Pages是什么？

GitHub Pages 本用于介绍托管在 GitHub 的项目， 不过，由于他的空间免费稳定，用来做搭建一个博客再好不过了。

github Pages 可以被认为是用户编写的、托管在 github上 的静态网页。

## 注册GitHub

访问：http://www.github.com/

注册你的username和邮箱，邮箱十分重要，GitHub上很多通知都是通过邮箱的。

## 配置 SSH keys

我们如何让本地git项目与远程的github建立联系呢？用SSH keys。

### 检查 SSH keys的设置

首先我们需要检查你电脑上现有的ssh key：

	$ cd ~/.ssh 检查本机的ssh密钥

如果提示：`No such file or directory` 说明你是第一次使用git。

### 生成新的SSH Key：

	$ ssh-keygen -t rsa -C "邮件地址@youremail.com"

	Generating public/private rsa key pair.
	Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>

注意1: 此处的邮箱地址，你可以输入自己的邮箱地址；

注意2: 此处的「-C」的是大写的「C」

然后系统会要你输入密码：

	Enter passphrase (empty for no passphrase):<输入加密串>
	Enter same passphrase again:<再次输入加密串>

在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。这个设置是防止别人往你的项目里提交内容。

注意：输入密码的时候没有*字样的，你直接输入就可以了。

最后看到符号图案界面，就成功设置ssh key了：

### 添加 SSH Key 到 GitHub

在本机设置 SSH Key 之后，需要添加到 GitHub 上，以完成SSH链接的设置。

1、打开本地 `C:\Documents and Settings\Administrator.ssh\id_rsa.pub` 文件。此文件里面内容为刚才生成人密钥。如果看不到这个文件，你需要设置显示隐藏文件。准确的复制这个文件的内容，才能保证设置的成功。

2、登陆 github 系统。点击右上角的 `Account Settings` —> `SSH Public keys` —> `add another public keys`

3、把你本地生成的密钥复制到里面（key文本框中）， 点击 `add key` 就ok了

### 测试

可以输入下面的命令，看看设置是否成功，`git@github.com` 的部分不要修改：

	$ ssh -T git@github.com

如果是下面的反馈：

	The authenticity of host 'github.com (207.97.227.239)' can't be established.
	RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
	Are you sure you want to continue connecting (yes/no)?

不要紧张，输入 `yes` 就好，然后会看到：

	Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.

## 设置用户信息

现在你已经可以通过 SSH 链接到 GitHub 了，还有一些个人信息需要完善的。

Git 会根据用户的名字和邮箱来记录提交。GitHub 也是用这些信息来做权限的处理，输入下面的代码进行个人信息的设置，把名称和邮箱替换成你自己的，名字必须是你的真名，而不是GitHub的昵称。

	$ git config --global user.name "xxx"//用户名
	$ git config --global user.email  "cnfeat@gmail.com"//填写自己的邮箱

SSH Key 配置成功本机已成功连接到 github。

若有问题，请重新设置。

使用 GitHub Pages 建立博客与 GitHub 建立好链接之后，就可以方便的使用它提供的 Pages 服务。

### GitHub Pages 分两种：

一种是你的 GitHub 用户名建立的 `username.github.io` 这样的用户&组织页（站）；

另一种是依附项目的 pages 。

想建立个人博客是用的第一种，形如 `xxx.github.io` 这样的可访问的站，每个用户名下面只能建立一个。


## 如何更新博文

下载博客模板的 ZIP去到你 frok 的仓库地址：`https://github.com/你的用户名/你的用户名.github.io`

点击右下角的 `Download ZIP`，你会得到一个名为`「你的用户名.github.io-master.zip」`的压缩包。

解压到本地，将文件夹改名为’`你的 github 用户名.github.io`’

## 安装 github desktop 

下载地址：[GitHub Desktop](https://desktop.github.com/)

打开 github desktop 软件，点击左上角的「+」号，选择 `add`

选择 ‘`你的 github 用户名.github.io`’ 文件夹。

如此，你的本地博客仓库就已经和 github 的仓库同步起来了。