---
layout: post
title: phpMyAdmin访问远程MySQL数据库的方法
date: 2017-01-03
categories: blog
tags: [mysql]
description: phpMyAdmin访问远程MySQL数据库的方法
---

## MySQL服务器配置允许远程连接

本地phpmyadmin远程连接服务器端MySQL，首先要确定你的mysql远程连接已开启，如果没有开启按照下面的二个方法操作：

### 方法一：改表法

因为在linux环境下，默认是关闭3306端口远程连接的，需要开启，如果你装mysql数据库时开启了3306就OK了。
默认mysql帐号不允许从远程登陆，只允许localhost访问。

登入mysql后，更改 "mysql" 数据库 里的 "user"（远程数据库的名称） 表里的 "host" 项，把"localhost"改称"%" 。这样你的mysql就可以远程操作了。

`update user set host = '%' where user = 'root';`   

注意：这样方法只是把本机localhost访问改为了"%"所有地址IP都可以访问mysql服务器，这样很不安全。默认localhost访问的时候有所有操作权限。所以不安全！推荐用下面的第二个方法。

### 方法二：授权法 (推荐使用)

#### (1) SQL语句:

`grant select,insert,update,delete on *.* to root@"%" Identified by "password";`

允许地址IP上root用户，密码dboomysql来连接mysql的所有数据库，只给select,insert,update,delete权限。 这样比较安全。如果只允许IP（192.168.1.1）上root用户访问更安全，操作(2)

#### (2) SQ语句：

`grant select,insert,update,delete on *.* to root@"192.168.1.1" Identified by "password";`

只允许地址IP（192.168.1.1）上root用户访问更安全了。

#### (3) SQ语句：

`grant all on *.* to root@"192.168.1.1" Identified by "password"`

允许地址192.168.1.1上用root用户，密码password来连接mysql的所有数据库，给所有权限。不太安全。


现在重启mysql服务，如果你的服务器上安装了防火墙，看看3306端口开启没，如果没有，需要开启3306端口才能用。

#### 在linux下要开启防火墙 打开3306 端口

编辑这个文件

`vi /etc/sysconfig/iptables`

输入

`-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT`

保存后在控制台输入

`/etc/init.d/iptables restart`

重启防火墙，记得**一定要重启防火墙**。

 
## 配置好本地 PHP 环境

### 方法一：

#### 一、 下载 phpmyadmin

[http://www.phpmyadmin.net/home_page/index.php](http://www.phpmyadmin.net/home_page/index.php)

#### 二、 修改 libraries 文件夹下的 config.default.php 文件或者 phpmyadmin 根目录的 config.inc.php文件。

1、查找 `$cfg['PmaAbsoluteUri']` ，将其值设置为你本地的 phpmyadmin 路径

2、查找 `$cfg['Servers'][$i]['host']` ， 将其值设置为你 mysql 数据库地址，例如 127.0.0.1

3、查找 `$cfg['Servers'][$i]['user']` ， 将其值设置为你 mysql 数据库用户名，例如 admin

4、查找 `$cfg['Servers'][$i]['password']` ， 将其值设置为你 mysql 数据库密码，例如 admin

#### 三、 phpMyAdmin只需要修改 $cfg['Servers'][$i]['host']的值，用户名密码 在访问 phpmyadmin 时输入即可。

`$cfg['Servers'][$i]['host'] = '192.168.16.253';`

### 方法二：

1，在浏览器中输入：`http://localhost/phpmyadmin/setup/`

2，点击“新建服务器” ，填写表单：

    服务器名称:主机名称
    服务器主机名:主机IP
    认证方式:config
    config 认证方式的用户名:用户名
    config 认证方式的密码：用户密码

3，保存后返回 setup 界面，点击下载，下载的文件为 `config.inc.php`

4，将下载的文件 `config.inc.php` 复制到 `/usr/share/phpmyadmin/` 目录下
 
5，编辑 `/usr/share/phpmyadmin/config.inc.php`

添加以下代码

    /* Server: localhost [1] */
    $i++;
    $cfg['Servers'][$i]['verbose'] = '主机名称';
    $cfg['Servers'][$i]['host'] = '主机IP';
    $cfg['Servers'][$i]['port'] = '';
    $cfg['Servers'][$i]['socket'] = '';
    $cfg['Servers'][$i]['connect_type'] = 'tcp';
    $cfg['Servers'][$i]['extension'] = 'mysqli';
    $cfg['Servers'][$i]['auth_type'] = 'config';
    $cfg['Servers'][$i]['user'] = '用户名';
    $cfg['Servers'][$i]['password'] = '密码';