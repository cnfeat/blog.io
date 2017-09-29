---
layout: post
title: CentOS下更改nginx默认的网页目录
date: 2017-01-23
categories: blog
tags: [nginx]
description: CentOS下更改nginx默认的网页目录
---

Nginx默认网站根目录：

    /usr/share/nginx/html  或  /usr/local/nginx/html

Nginx默认配置文件位置：

    /etc/nginx/nginx.conf  或  /usr/local/nginx/conf/nginx.conf

(PS: 本人虚拟机安装CentOS 6.7，Nginx 1.6.2 环境下，默认文件位置：`/etc/nginx/conf.d/default.conf` )

(查看 Nginx 版本命令：`nginx -v` )

### 如要将默认网页目录改成  /home/www：
 
将配置文件 nginx.conf 中的
 
        location / {
            root   html;
            index  index.php index.html index.htm;
        }
 
改为
 
        location / {
            root   /home/www;
            index  index.php index.html index.htm;
        }
 
然后再将
 
    location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
    }
 
改为
 
    location ~ \.php$ {
            root           /home/www;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
    }
 
然后重启nginx

    service nginx restart
 