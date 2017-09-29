---
layout: post
title: Linux下PhpMyAdmin程序目录结构的安全管理
date: 2017-05-19
categories: blog
tags: [linux]
description: Linux下PhpMyAdmin程序目录结构的安全管理
---

PhpMyAdmin是一套放在服务器端的通过浏览器界面管理数据库的程序，因此，确保其目录安全性十分重要，否则，将导致数据被盗取甚至遭到恶意破坏。下面将详细讲述一般的防范措施。 

### 一、 修改phpMyAdmin目录名： 

在不修改目录名前，其他人很容易猜到该目录名，造成安全隐患。

如，假设一台Linux主机的域名为：`www.test.com`，那么不修改目录名的情况下，在地址栏中输入：`www.test.com/phpMyAdmin/` 就将进入 `phpMyAdmin` 管理程序。

因此如果将 `phpMyAdmin` 目录改名为一个别人不易知道的目录，如 `mynameadmin`，这样，你在管理自己的数据库时，只要键入：`www.test.com/mynameadmin/` 就可以通过浏览器管理数据库了。

（注：下面仍将使用 `phpMyAdmin` 目录名，如果目录名已换，只需把 `phpMyAdmin` 改名为新的目录名即可。） 

### 二、 对phpMyAdmin目录加用户身份验证： 

这是很多网站需要用户验证时普遍使用的方法，这样当用户第一次浏览进入该目录时，都将出现一个提示窗口，提示用户输入用户名和密码验证。

是通过使用 Apache Server 的标准  mod_auth 模块实现的，具体操作方法如下： 

#### 1、VI编辑 Apache Server 配置文件。

确保文件中如下两句话没有加注释，如果这两句话前有"#"符号，去掉"#"号。 

	DocumentRoot /data/web/apache/public/htdocs 
	AccessFileName . htaccess 
	AllOerride All 

#### 2、passwd 程序创建用户文件： 

	htpasswd - c /data/web/apache/secrects/.htpasswd 88998 

其中，`-c` 表示选项告诉 `htpasswd` 你想生成一个新的用户文件，

`/data/web/apache/secrects/` 是你想存放 `.htpasswd` 文件的目录，

文件名称为 `.htpasswd`，

`88998` 是在验证时所用到的用户名，

敲如以上命令后，系统提示你输入密码，这个密码就是验证时所需要用到的密码，该密码在 `.htpasswd` 文件中是加密的。

现在用 `more` 来查看 `/data/web/apache/secrects/.htpasswd` 文件，可以看到其中有一行用户名和一串加密密码。 

#### 3、创建 .htaccess 文件： 

使用文本编辑器，在目录 `phpMyAdmin` （如果已经改名，就是新的目录名）下创建 `.htaccess` 文件，在文件中加入如下语句： 

	AuthName "用户验证" 
	AuthType Basic 
	AuthUserFile /data/web/apache/public/htdocs/phpMyAdmin/.htpasswd 
	require user 88998 

保存所做操作后，再去看 `phpMyAdmin` 目录，将提示验证窗口，输入刚用 `htpasswd` 命令创建的用户名和密码，即可进入该目录。 

### 三、 增加基于主机的访问控制： 

在修改了目录名和增加访问验证机制后，应该说现在的 `phpMyAdmin` 已经很安全了，但由于 `phpMyAdmin` 目录一般只是数据库管理员使用，为防止别人还知道目录名称和验证密码，还可以增加如下的基于主机的访问控制。

基于主机的访问是通过验证用户机器 `IP` 来实现的，即只有符合条件的 `IP` 才可以反问该目录，否则拒绝访问。 

修改 `.htaccess` 文件如下： 

	AuthName "用户验证" 
	AuthType Basic 
	AuthUserFile /data/web/apache/public/htdocs/phpMyAdmin/.htpasswd 
	require user 88998 

	order deny,allow 
	deny from all 
	allow from 202.100.222.80 

这里增加了三条基于主机访问控制指令，

其中第一条 `order` 指令的值是由一个逗号隔开的名单，这个名单表明了**哪一个指令更高的优先权**，

第二条指令 `deny` 定义不能访问该目录的主机，

第三条指令 `allow` 定义可以访问该目录的主机，

这样，该目录除了IP地址为 202.100.222.80 的机器可以访问该目录之外，其他的都不能访问，读者可以把该地址改为**用户数据库管理员IP**。 

### 总结：

通过以上三点相结合，就可很好的确保 `phpMyAdmin` 目录的安全，非数据库管理员将很难通过 `phpMyAdmin` 程序读取数据。

这里所讲的是针对于 `phpMyAdmin` 目录进行讲述，其他目录如需加访问限制，也可依此方法操作。