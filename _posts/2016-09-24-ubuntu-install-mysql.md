---
layout: post
title: Ubuntu 安装 mysql 服务
date: 2016-09-24
categories: blog
tags: [ubuntu]
description: Ubuntu 安装 mysql 服务

---

### ubuntu上安装mysql非常简单只需要几条命令就可以完成。

    sudo apt-get install mysql-server 
    apt-get install mysql-client 
    sudo apt-get install libmysqlclient-dev
 
安装过程中会提示设置密码什么的，注意设置了不要忘了。


### 安装完成之后可以使用如下命令来检查是否安装成功：
 
`sudo netstat -tap | grep mysql`
 
通过上述命令检查之后，如果看到有mysql 的socket处于 listen 状态则表示安装成功。

 
### 登陆mysql数据库可以通过如下命令：
 
`mysql -u root -p` 
 
-u 表示选择登陆的用户名， -p 表示登陆的用户密码

上面命令输入之后会提示输入密码，此时输入密码就可以登录到mysql。