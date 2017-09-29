---
layout: post
title: 关于linux下php权限问题
date: 2017-02-18
categories: blog
tags: [php]
description: 关于linux下php权限问题
---

### 要设置安全的权限系统，你要先弄清楚以下几个问题：

1. 网站的文件所有者帐号是什么？
2. `apache/php-fpm` 以什么帐号身份运行？
3. 网站哪些目录需要有写入权限（如日志生成、附件上传等）

### 针对这个问题，建议的设置如下：

1. 网站所有者，可设置为 ftp, www 帐号
2. `nginx/php-fpm/apache`，建议以 nobody 帐号运行，反正不能使用网站文件所有者帐号。
3. 需要可写权限的目录，手工设置权限为 777 即可
4. php生成的日志、附件文件的所有者会是 nobody, 这时 www,ftp 帐号却无法修改、删除这些文件。那么在php生成文件时，可调用 `chmod($filename, 0777)`。即解钤还需系钤人。

这样，php脚本只能向指定的目录中写入文件，一方面规范了程序代码的行为，另一方面，也一定程度上提高了网站的安装性。

Nginx和PHP-FPM运行用户都使用Nginx，这个用户是一个不能登录的普通用户， yum安装时会自动新建，自己编译安装的话需要手动新建：

	addgroup nginx --system
	adduser nginx --system --disabled-login --ingroup nginx --no-create-home --home /nonexistent --gecos "nginx user" --shell /bin/false

网站根目录的所有者**不能**设为Nginx用户，可以是你上传文件的用户。

### 网站目录基本的权限设置(目录755,文件644)：

	find -type d -exec chmod 755 {} \;
	find -type f -exec chmod 644 {} \;

也就是说 Nginx 和 PHP-FPM 只能读文件和访问目录。

对于特别的目录，比如文件上传目录，需要给 Nginx 写权限，这时可以设为 777：

`chmod 777 www/uploads`

但注意配置该目录不做PHP解析，比如：

	location ~ \.php$ {
        include fastcgi_params;
        if ($uri !~ "^/uploads/") {
        fastcgi_pass 127.0.0.1:9000;
        }
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME /path/to/www$fastcgi_script_name;
    }

另外还有像 **缓存目录** 需要写权限的目录同样可以这样配置。 

其实PHP可以很安全，只要你想去保护她。