---
layout: post
title: 如何安装nginx第三方模块
date: 2017-01-25
categories: blog
tags: [nginx]
description: 如何安装nginx第三方模块
---

nginx文件非常小但是性能非常的高效，这方面完胜apache,nginx文件小的一个原因之一是nginx自带的功能相对较少,好在nginx允许第三方模块，第三方模块使得nginx越发的强大。

在安装模块方面，nginx显得没有apache安装模块方便，当然也没有php安装扩展方便。在原生的nginx，他不可以动态加载模块，所以当你安装第三方模块的时候需要覆盖nginx文件.接下来看看如何安装nginx第三模块吧。

### nginx第三方模块安装方法：
 
    ./configure--prefix=/你的安装目录  --add-module=/第三方模块目录
 
以安装pagespeed模块实例

#### 1、在未安装nginx的情况下安装nginx第三方模块
 
    # ./configure --prefix=/usr/local/nginx-1.4.1 \
    --with-http_stub_status_module\
    --with-http_ssl_module--with-http_realip_module\
    --with-http_image_filter_module\
    --add-module=../ngx_pagespeed-master--add-module=/第三方模块目录
    # make
    # make install
    # /usr/local/nginx-1.4.1/sbin/nginx
 
#### 2、在已安装nginx情况下安装nginx模块 

    # ./configure --prefix=/usr/local/nginx-1.4.1 \
    --with-http_stub_status_module\
    --with-http_ssl_module--with-http_realip_module\
    --with-http_image_filter_module\
    --add-module=../ngx_pagespeed-master
    # make
    # /usr/local/nginx-1.4.1/sbin/nginx -s stop

    # cp objs/nginx /usr/local/nginx/sbin/nginx  （多了这一步）

    # /usr/local/nginx-1.4.1/sbin/nginx
 
相比之下仅仅多了一步覆盖nginx文件。

**总结：**安装nginx安装第三方模块实际上是使用--add-module重新安装一次nginx，不要make install而是直接把编译目录下objs/nginx文件直接覆盖老的nginx文件。如果你需要安装多个nginx第三方模块,你只需要多指定几个相应的--add-module即可。

**备注：**重新编译的时候，记得一定要把以前编译过的模块一同加到configure参数里面。

nginx提供了非常多的nginx第三方模块提供安装，地址 [http://wiki.nginx.org/3rdPartyModules](http://wiki.nginx.org/3rdPartyModules)
 