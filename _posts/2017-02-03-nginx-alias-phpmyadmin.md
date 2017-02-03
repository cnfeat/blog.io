---
layout: post
title: Nginx设置alias别名目录访问phpmyadmin
date: 2017-02-03
categories: blog
tags: [nginx]
description: Nginx设置alias别名目录访问phpmyadmin
---

### 引言：

Nginx服务器通过设置alias别名可以使特定的目录（phpmyadmin目录）不出现在网站根目录下面，即使网站根目录被攻破，也不会影响到phpmyadmin目录里面的文件。

### 说明：

站点：http://192.168.16.254/

站点根目录：/home/www

Nginx运行账户：nginx

Nginx运行账户组：nginx

phpmyadmin目录：/home/phpmyadmin

MySQL用户名：root

密码：123456

### 实现目的：

通过访问这个地址，实现对MySQL数据库的管理

### 操作步骤

#### 1、下载phpmyadmin

    cd /home
    wget https://www.phpmyadmin.net/files/xxx  #下载
    tar xvfz phpMyAdmin-3.5.1-all-languages.tar.gz  #解压
    mv phpMyAdmin-3.5.1-all-languages phpmyadmin #更改文件夹名字为phpmyadmin

#### 2、修改nginx配置文件

    cp /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.bak #备份配置文件
    vi /etc/nginx/conf.d/default.conf #修改配置文件，在
    server {
        listen 80;
        server_name localhost;
        location / {
        root html;
        index index.php index.html index.htm;
    }
    下面添加以下内容：
    location /phpmyadmin {
        alias /home/phpmyadmin;
        index index.php;
    }
    location ~ /phpmyadmin/.+\.php$ {
        if ($fastcgi_script_name ~ /phpmyadmin/(.+\.php.*)$) {
            set $valid_fastcgi_script_name $1;
        }
        include fastcgi_params;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME /home/phpmyadmin/$valid_fastcgi_script_name;
    }
    :wq! #保存，退出

注：新增加的location段必须放在 location ~ \.php$ {}  段前,否则url打不开

#### 3、设置/home/phpmyadmin目录权限

    chown nginx.nginx /home/phpmyadmin -R  #修改目录所有者为nginx账号
    service nginx restart  #重启nginx
    service php-fpm restart  #重启php-fpm

#### 4、现在可以使用域名+phpmyadmin来访问了

http://192.168.16.254/phpmyadmin/，出现登录界面

输入MySQL的账号密码，点执行，即可登录到phpmyadmin的管理界面

至此，Nginx设置alias别名目录访问phpmyadmin教程完成

### 附：

phpmyadmin下载：[https://www.phpmyadmin.net/files/](https://www.phpmyadmin.net/files/)