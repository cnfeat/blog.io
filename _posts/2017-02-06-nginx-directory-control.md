---
layout: post
title: nginx目录列表和目录访问权限设置
date: 2017-02-06
categories: blog
tags: [nginx]
description: nginx目录列表和目录访问权限设置
---

## 1.目录列表(directory listing)

nginx让目录中的文件以列表的形式展现只需要一条指令，这样就不会返回403错误了：

`autoindex on;`

autoindex 可以放在 **location** 中，只对 **当前location的目录** 起作用。

你也可以将它放在 **server指令块** 则对 **整个站点** 都起作用。

或者放到 **http指令块**，则对 **所有站点** 都生效。

下面是一个简单的例子:

    server {
            listen   80;
            server_name  domain.com www.domain.com;
            access_log  /var/...........................;
            root   /var/www/html;
            location / {
                    index  index.php index.html index.htm;
            }
            location /api {
                   autoindex on;
            }
    }

## 2.nginx禁止访问某个目录

跟Apache的`Deny from all`类似，nginx有`deny all`指令来实现。

### 禁止对叫 dirdeny目录 的访问并返回 403 Forbidden，可以使用下面的配置:

    location /dirdeny {
        deny all;
        return 403;
    }

### nginx禁止访问path目录

    location ^~ /path {
        deny all;
    }

可以把path换成实际需要的目录，

目录path后是否带有"/",

**带"/"，只禁止访问目录，**

**不带"/"，禁止访问目录中的文件**

### nginx禁止访问所有.开头的隐藏文件设置

    location ~* /.* {
        deny all;
    }


`autoindex_exact_size off;`  //人性化方式显示文件大小否则以byte显示

`autoindex_localtime on;`  //按服务器时间显示，否则以gmt时间显示，gmt时间指格林尼治时间

`autoindex_exact_size off;`

默认为on，显示出文件的确切大小，单位是bytes。

改为off后，显示出文件的大概大小，单位是kB或者MB或者GB

`autoindex_localtime on;`

默认为off，显示的文件时间为GMT时间。

改为on后，显示的文件时间为文件的服务器时间