---
layout: post
title: Nginx错误原因和解决方法总结
date: 2017-07-27
categories: blog
tags: [nginx]
description: Nginx错误原因和解决方法总结
---

## 一、NGINX 502/504 错误分析

`Nginx 502 Bad Gateway` 的含义是请求的 PHP-CGI 已经执行，但是由于某种原因（一般是读取资源的问题）没有执行完毕而导致 PHP-CGI 进程终止。

`Nginx 504 Gateway Time-out` 的含义是所请求的网关没有请求到，简单来说就是没有请求到可以执行的 PHP-CGI 。

解决这两个问题其实是需要综合思考的，一般来说 `Nginx 502 Bad Gateway` 和 `php-fpm.conf` 的设置有关，而 `Nginx 504 Gateway Time-out` 则是与 `nginx.conf` 的设置有关。

而正确的设置需要考虑服务器自身的性能和访客的数量等多重因素。

下面我们来仔细分析一下 `php-fpm.conf` 几个重要的参数：

`php-fpm.conf` 有两个至关重要的参数，一个是” `max_children` ”, 另一个是” `request_terminate_timeout` ”

这个值不是通用的，而是需要自己计算的。

#### 计算的方式如下：

如果你的服务器性能足够好，且宽带资源足够充足，PHP脚本没有系循环或BUG的话你可以直接将”`request_terminate_timeout`”设置成 `0s` 。`0s`的含义是让 PHP-CGI 一直执行下去而没有时间限制。

而如果你做不到这一点，也就是说你的 PHP-CGI 可能出现某个BUG，或者你的宽带不够充足或者其他的原因导致你的 PHP-CGI 能够假死那么就建议你给” `request_terminate_timeout` ”赋一个值，这个值可以根据你服务器的性能进行设定。一般来说性能越好你可以设置越高，20分钟-30分钟都可以。

而” `max_children` ”这个值又是怎么计算出来的呢？

这个值原则上是越大越好，php-cgi的进程多了就会处理的很快，排队的请求就会很少。

设置” `max_children` ”也需要根据服务器的性能进行设定(假设服务器内存`1GB`)，一般来说一台服务器正常情况下每一个 php-cgi 所耗费的内存在 `20M` 左右，因此” `max_children` ”设置成 `40` 个，`20M*40=800M` 也就是说在峰值的时候所有 PHP-CGI 所耗内存在 `800M` 以内，低于有效内存1Gb。

而如果”` max_children` ”设置的较小，比如 `5-10`个，那么 php-cgi 就会处理速度也很慢，等待的时间也较长。

如果长时间没有得到处理的请求就会出现 `504 Gateway Time-out` 这个错误，而正在处理的那几个 `php-cgi` 如果遇到了问题就会出现5 `02 Bad gateway` 这个错误。

----------

## 二、NGINX 502 错误排查

NGINX 502 Bad Gateway 错误是 FastCGI 有问题，造成 NGINX 502 错误的可能性比较多。将网上找到的一些和502 Bad Gateway错误有关的问题和排查方法列一下，先从FastCGI配置入手：

#### 1.FastCGI进程是否已经启动

#### 2.FastCGI worker进程数是否不够

运行 

`netstat -anpo | grep “php-cgi” | wc -l` 

判断是否接近FastCGI进程，接近配置文件中设置的数值，表明worker进程数设置太少

#### 3.FastCGI执行时间过长

根据实际情况调高以下参数值

	fastcgi_connect_timeout 300;

	fastcgi_send_timeout 300;

	fastcgi_read_timeout 300;

#### 4.FastCGI Buffer不够

nginx 和 apache 一样，有前端缓冲限制，可以调整缓冲参数

	fastcgi_buffer_size 32k;

	fastcgi_buffers 8 32k;

#### 5.Proxy Buffer不够

如果你用了Proxying，调整

	proxy_buffer_size   16k;

	proxy_buffers    4 16k;

#### 6.https转发配置错误

正确的配置方法

	server_name www.mydomain.com;

	location /myproj/repos {

		set $fixed_destination $http_destination;

		if ( $http_destination ~* ^https(.*)$ )

			{

			set $fixed_destination http$1;

			}

		proxy_set_header Host $host;

		proxy_set_header X-Real-IP $remote_addr;

		proxy_set_header Destination $fixed_destination;

		proxy_pass http://subversion_hosts;

	}

当然，还要看你后端用的是哪种类型的 FastCGI，我用过的有 php-fpm，流量约为单台机器40万PV(动态页面), 现在基本上没有碰到502。

#### 7.php脚本执行时间过长

将 `php-fpm.conf` 的

	<value name="request_terminate_timeout">0s</value>

的`0s`改成一个时间

## 三、Nginx 413 错误的排查: 修改上传文件大小限制

在上传时 nginx 返回了 413 错误。

查看 log 文件，显示的错误信息是: `”413 Request Entity Too Large”`。

在网上找了下“`nginx 413错误`”发现需要做以下设置：

在 `nginx.conf` 增加

	client_max_body_size

的相关设置, 这个值默认是 `1m`，可以增加到 `8m` 以增加提高文件大小限制；

如果运行的是 php，那么还要检查 `php.ini`，这个大小 `client_max_body_size` 要和 `php.ini` 中的如下值的最大值一致或者稍大，这样就不会因为提交数据大小不一致出现的错误。

	post_max_size = 8M

	upload_max_filesize = 2M

## 四、Nginx 400 错误排查：HTTP 头 / Cookie 过大

今天有人汇报 nginx 的 HTTP 400 错误，而且这个 HTTP 400 错误并不是每次都会出现的。

查了一下发现 nginx 400 错误是由于 `request header` 过大，通常是由于 cookie 中写入了较长的字符串所引起的。

#### 解决方法是：

不要在 cookie 里记录过多数据，如果实在需要的话可以考虑调整在 `nginx.conf` 中的 `client_header_buffer_size` (默认`1k`)

若 cookie 太大，可能还需要调整 `large_client_header_buffers` (默认`4k`)，该参数说明如下：

请求行如果超过 buffer，就会报 `HTTP 414 `错误(`URI Too Long`)

nginx 接受最长的HTTP头部大小必须比其中一个 buffer 大，否则就会报 400 的 HTTP 错误(`Bad Request`)。