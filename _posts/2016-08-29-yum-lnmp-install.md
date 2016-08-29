---
layout: post
title: CentOS 6.5 yum安装配置lnmp服务器(Nginx+PHP+MySQL)
date: 2016-08-29
categories: blog
tags: [lnmp,centos]
description: CentOS 6.5 yum安装配置lnmp服务器(Nginx+PHP+MySQL)

---

## 准备篇：

### 1、配置防火墙，开启80端口、3306端口
`vi /etc/sysconfig/iptables`

<pre><code>
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT（允许80端口通过防火墙）
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT（允许3306端口通过防火墙）
</code></pre>

特别提示：很多网友把这两条规则添加到防火墙配置的最后一行，导致防火墙启动失败，正确的应该是添加到默认的22端口这条规则的下面
添加好之后防火墙规则如下所示：

<pre><code>
#########################################################
# Firewall configuration written by system-config-firewall
# Manual customization of this file is not recommended.
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -mstate --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -mstate --state NEW -m tcp -p tcp --dport 3306 -j ACCEPTy
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
#########################################################
</code></pre>

`/etc/init.d/iptables restart    #最后重启防火墙使配置生效`

### 2、关闭SELINUX
`vi /etc/selinux/config`

<pre><code>
 #SELINUX=enforcing       #注释掉
 #SELINUXTYPE=targeted    #注释掉
 SELINUX=disabled         #增加
</code></pre>

`:wq  保存，关闭`

`shutdown -r now   #重启系统`

### 3、配置CentOS 6.2第三方yum源（CentOS默认的标准源里没有nginx软件包）
`yum install wget    #安装下载工具wget`

`wget http://www.atomicorp.com/installers/atomic  #下载atomic yum源`

`sh ./atomic   #安装`

`yum check-update  #更新yum软件包`

#############################################################################

## 安装篇：

### 一、安装nginx

`yum install nginx     #安装nginx，根据提示，输入Y安装即可成功安装`

`service nginx start   #启动`

`chkconfig nginx on    #设为开机启动(服务自动运行)`

`/etc/init.d/nginx  restart  #重启`

`rm -rf /usr/share/nginx/html/*  #删除ngin默认测试页`


### 二、安装MySQL

#### 1、安装mysql

`yum install mysql mysql-server   #询问是否要安装，输入Y即可自动安装,直到安装完成`

`/etc/init.d/mysqld start   #启动MySQL`

`chkconfig mysqld on   #设为开机启动`

`cp /usr/share/mysql/my-medium.cnf   /etc/my.cnf  #拷贝配置文件（注意：如果/etc目录下面默认有一个my.cnf，直接覆盖即可）`

`cp /usr/share/mysql/my-innodb-heavy-4G.cnf  /etc/my.cnf`

PS:/usr/share/mysql/ 下有好几种默认的配置文件，按需要选择。我们公司选择用 my-innodb-heavy-4G.cnf

注：如果修改过my.cnf，会覆盖原配置文件！

`shutdown -r now  #重启系统`

#### 2、为root账户设置密码

`mysql_secure_installation`

回车，根据提示输入Y

输入2次密码，回车

根据提示一路输入Y

最后出现：Thanks for using MySQL!

MySql密码设置完成

重新启动 MySQL：

`/etc/init.d/mysqld stop   #停止`

`/etc/init.d/mysqld start  #启动`

`service mysqld restart    #重启`

### 三、安装PHP

#### 1、安装PHP

`yum install php   #根据提示输入Y直到安装完成`

#### 2、安装PHP组件，使PHP支持 MySQL、PHP支持FastCGI模式

`yum install php-mysql php-gd libjpeg* php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-mcrypt php-bcmath php-mhash libmcrypt libmcrypt-devel php-fpm          #根据提示输入Y回车`

`/etc/init.d/mysqld restart  #重启MySql`

`/etc/init.d/nginx  restart  #重启nginx`

`/etc/rc.d/init.d/php-fpm start  #启动php-fpm`

`chkconfig php-fpm on  #设置开机启动`

#############################################################################

## 配置篇

### 一、配置nginx支持php

`cp /etc/nginx/nginx.conf  /etc/nginx/nginx.conf.bak    #备份原有配置文件`

`vi /etc/nginx/nginx.conf  #编辑`

`user  nginx  nginx;  #修改nginx运行账号为：nginx组的nginx用户`

`:wq!    #保存退出`

`cp /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.bak   #备份原有配置文件`

`vi /etc/nginx/conf.d/default.conf   #编辑`

<pre><code>
index index.php index.html index.htm;   #增加index.php
 # pass the PHPscripts to FastCGI server listening on 127.0.0.1:9000
 #
 location ~ \.php$ {
   root          html;
   fastcgi_pass   127.0.0.1:9000;
   fastcgi_index  index.php;
   fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
   include       fastcgi_params;
 }

#取消FastCGI server部分location的注释,并要注意fastcgi_param行的参数,改为$document_root$fastcgi_script_name,或者使用绝对路径
</code></pre>

### 二、配置php

`cp /etc/php.ini /etc/php.ini.bak`

`vi /etc/php.ini   #编辑`

<pre><code>
date.timezone= PRC     #在946行 把前面的分号去掉，改为date.timezone = PRC
 
disable_functions=passthru,exec,system,chroot,scandir,chgrp,chown,shell_exec,proc_open,proc_get_status,ini_alter,ini_restore,dl,openlog,syslog,readlink,symlink,popepassthru,stream_socket_server,escapeshellcmd,dll,popen,disk_free_space,checkdnsrr,getservbyname,getservbyport,disk_total_space,posix_ctermid,posix_get_last_error,posix_getcwd,posix_getegid,posix_geteuid,posix_getgid,posix_getgrgid,posix_getgrnam,posix_getgroups,posix_getlogin,posix_getpgid,posix_getpgrp,posix_getpid,posix_getppid,posix_getpwnam,posix_getpwuid,posix_getrlimit,posix_getsid,posix_getuid,posix_isatty,posix_kill,posix_mkfifo,posix_setegid,posix_seteuid,posix_setgid,posix_setpgid,posix_setsid,posix_setuid,posix_strerror,posix_times,posix_ttyname,posix_uname
 
#在386行 列出PHP可以禁用的函数，如果某些程序需要用到这个函数，可以删除，取消禁用。
expose_php = Off        #在432行 禁止显示php版本的信息
magic_quotes_gpc = On   #在745行 打开magic_quotes_gpc来防止SQL注入
open_basedir = .:/tmp/  #在380行，设置表示允许访问当前目录(即PHP脚本文件所在之目录)和/tmp/目录,可以防止php木马跨站，如果改了之后安装程序有问题，可注销此行，或者直接写上程序目录路径/var/www/html/:/tmp/

注：加了可能导致phpmyadmin读取文件错误，无法使用。

</code></pre>

`:wq! #保存退出`

(注:magic_quotes_gpc我的配置中是不存在的,open_basedir没有看懂也就跳过了,这两条并没有影响我配置成功,隐患...暂时还没找到或者还没理解)

### 三、配置php-fpm

`cp /etc/php-fpm.d/www.conf   /etc/php-fpm.d/www.conf.bak   #备份原有配置文件`

`vi /etc/php-fpm.d/www.conf   #编辑`

<pre><code>
user = nginx   #修改用户为nginx
group = nginx   #修改组为nginx
</code></pre>

`/etc/init.d/mysqld restart  #重启MySql`

`/etc/init.d/nginx  restart  #重启nginx`

`/etc/rc.d/init.d/php-fpm  restart  #重启php-fpm`

#############################################################################

## 测试篇

`cd /usr/share/nginx/html/   #进入nginx默认网站根目录`

`vi  index.php   #新建index.php文件`

    <?php
     phpinfo();
    ?>


`:wq! #保存`

`chown nginx.nginx /usr/share/nginx/html/ -R  #设置目录所有者`

`chmod 700  /usr/share/nginx/html/ -R   #设置目录权限`

在客户端浏览器输入服务器IP地址，可以看到相关的配置信息！
#############################################################################