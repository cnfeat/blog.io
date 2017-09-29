---
layout: post
title: nginx的rewrite规则
date: 2017-07-25
categories: blog
tags: [nginx]
description: nginx的rewrite规则
---

nginx 的 rewrite 规则参考：

	1. ~ 为区分大小写匹配
	2. ~* 为不区分大小写匹配
	3. !~ 和 !~* 分别为 区分大小写不匹配 及 不区分大小写不匹配


	1. -f 和 !-f 用来判断是否存在文件
	2. -d 和 !-d 用来判断是否存在目录
	3. -e 和 !-e 用来判断是否存在文件或目录
	4. -x 和 !-x 用来判断文件是否可执行


	1. last 相当于Apache里的[L]标记，表示完成rewrite，这应该是最常用的
	2. break 终止匹配, 不再匹配后面的规则
	3. redirect 返回302临时重定向 地址栏会显示跳转后的地址
	4. permanent 返回301永久重定向 地址栏会显示跳转后的地址



### set指令

语法： `set variable value;`

默认值：`none`

作用域：`server`, `location`, `if`

定义一个变量并赋值，值可以是文本，变量或者文本变量混合体。

## Rewrite规则

**rewrite 功能就是，使用 nginx 提供的全局变量或自己设置的变量，结合正则表达式和标志位实现url重写以及重定向。**

rewrite 只能放在 `server{}`, `location{}`, `if{}` 中，并且**只能对域名后边的除去传递的参数外的字符串起作用。**

例如：`http://seanlook.com/a/we/index.php?id=1&u=str`

只对 `/a/we/index.php` 重写。

语法：

	rewrite regex replacement [flag];

如果想对域名或参数字符串起作用，可以使用全局变量匹配，也可以使用 proxy_pass 反向代理。

#### 表面上看 rewrite 和 location 功能有点像，都能实现跳转，主要区别在于：

rewrite 是在同一域名内更改获取资源的路径，

而 location 是对一类路径做控制访问或反向代理，可以 proxy_pass 到其他机器。

很多情况下 rewrite 也会写在 location 里，它们的执行顺序是：

1. 执行 server 块的 rewrite 指令
2. 执行 location 匹配
3. 执行选定的 location 中的 rewrite 指令

如果其中某步URI被重写，则重新循环执行1-3，直到找到真实存在的文件；循环超过10次，则返回500 Internal Server Error错误。

### 2.1 flag标志位

* last : 相当于 Apache 的 [L] 标记，表示完成 rewrite
* break : 停止执行当前虚拟主机的后续 rewrite 指令集
* redirect : 返回 302 临时重定向，地址栏会显示跳转前的地址
* permanent : 返回 301 永久重定向，地址栏会显示跳转后的地址

因为 **301 和 302 不能简单的只返回状态码，还必须有重定向的URL**，这就是 return 指令无法返回 301 , 302 的原因了。

#### 这里 last 和 break 区别有点难以理解：

1. last一般写在 server 和 if 中，而 break 一般使用在 location 中
2. last 不终止重写后的 url 匹配，即新的 url 会再从 server 走一遍匹配流程，而 break 终止重写后的匹配
3. break 和 last 都能组织继续执行后面的 rewrite 指令

### 2.2 if 指令与全局变量

if 判断指令语法：

	if (condition) {...}

对给定的条件 condition 进行判断。如果为真，大括号内的 rewrite 指令将被执行，

if 条件 (conditon) 可以是如下任何内容：

* 当表达式只是一个变量时，如果值为空或任何以 0 开头的字符串都会当做 `false`
* 直接比较变量和内容时，使用 `=` 或 `!=`
* `~` 正则表达式匹配，`~*` 不区分大小写的匹配，`!~` 区分大小写的不匹配
* `-f` 和 `!-f` 用来判断是否存在文件
* `-d` 和 `!-d` 用来判断是否存在目录
* `-e` 和 `!-e` 用来判断是否存在文件或目录
* `-x` 和 `!-x` 用来判断文件是否可执行

#### 例如：


	if ($http_user_agent ~ MSIE) {
		rewrite ^(.*)$ /msie/$1 break;
	} 	//如果UA包含"MSIE"，rewrite请求到/msid/目录下

	if ($http_cookie ~* "id=([^;]+)(?:;|$)") {
		set $id $1;
	} 	//如果cookie匹配正则，设置变量$id等于正则引用部分

	if ($request_method = POST) {
		return 405;
	} 	//如果提交方法为POST，则返回状态405（Method not allowed）。return不能返回301,302

	if ($slow) {
		limit_rate 10k;
	} 	//限速，$slow可以通过 set 指令设置

	if (!-f $request_filename){
		break;
		proxy_pass http://127.0.0.1;
	} 	//如果请求的文件名不存在，则反向代理到localhost 。这里的break也是停止rewrite检查

	if ($args ~ post=140){
		rewrite ^ http://example.com/ permanent;
	} 	//如果query string中包含"post=140"，永久重定向到example.com

	location ~* \.(gif|jpg|png|swf|flv)$ {
		valid_referers none blocked www.jefflei.com www.leizhenfang.com;
		if ($invalid_referer) {
			return 404;
		} 	//防盗链
	}

### 全局变量

下面是可以用作if判断的全局变量

* `$args` ： #这个变量等于请求行中的参数，同 `$query_string`
* `$content_length` ： 请求头中的 Content-length 字段。
* `$content_type` ： 请求头中的 Content-Type 字段。
* `$document_root` ： 当前请求在 root 指令中指定的值。
* `$host` ： 请求主机头字段，否则为服务器名称。
* `$http_user_agent` ： 客户端 agent 信息
* `$http_cookie` ： 客户端 cookie 信息
* `$limit_rate` ： 这个变量可以限制连接速率。
* `$request_method` ： 客户端请求的动作，通常为 GET 或 POST。
* `$remote_addr` ： 客户端的IP地址。
* `$remote_port` ： 客户端的端口。
* `$remote_user` ： 已经经过 Auth Basic Module 验证的用户名。
* `$request_filename` ： **当前请求的文件路径，由root或alias指令与URI请求生成。**
* `$scheme` ： HTTP方法（如http，https）。
* `$server_protocol` ： 请求使用的协议，通常是 HTTP/1.0 或 HTTP/1.1。
* `$server_addr` ： 服务器地址，在完成一次系统调用后可以确定这个值。
* `$server_name` ： 服务器名称。
* `$server_port` ： 请求到达服务器的端口号。
* `$request_uri` ： 包含请求参数的原始URI，不包含主机名，如：”/foo/bar.php?arg=baz”。
* `$uri` ： 不带请求参数的当前URI，`$uri`不包含主机名，如”/foo/bar.html”。
* `$document_uri` ： 与$uri相同。

例：`http://localhost:88/test1/test2/test.php`


1. `$host`：localhost
2. `$server_port`：88
3. `$request_uri`：http://localhost:88/test1/test2/test.php
4. `$document_uri`：/test1/test2/test.php
5. `$document_root`：/var/www/html
6. `$request_filename`：/var/www/html/test1/test2/test.php

### 2.3 常用正则

* `.` ： 匹配除换行符以外的任意字符
* `?` ： 重复0次或1次
* `+` ： 重复1次或更多次
* `*` ： 重复0次或更多次
* `\d` ：匹配数字
* `^` ： 匹配字符串的开始
* `$` ： 匹配字符串的结束
* `{n}` ： 重复n次
* `{n,}` ： 重复n次或更多次
* `[c]` ： 匹配单个字符c
* `[a-z]` ： 匹配a-z小写字母的任意一个

小括号`()`之间匹配的内容，可以在后面通过`$1`来引用，`$2`表示的是前面第二个()里的内容。正则里面容易让人困惑的是`\`转义特殊字符。

### 2.4 rewrite实例

#### 例1：

	http {
		# 定义image日志格式
		log_format imagelog '[$time_local] ' $image_file ' ' $image_type ' ' $body_bytes_sent ' ' $status;

		# 开启重写日志
		rewrite_log on;

		server {
			root /home/www;

			location / {
				# 重写规则信息
				error_log logs/rewrite.log notice;
				# 注意这里要用‘’单引号引起来，避免{}
				rewrite '^/images/([a-z]{2})/([a-z0-9]{5})/(.*)\.(png|jpg|gif)$' /data?file=$3.$4;
				# 注意不能在上面这条规则后面加上“last”参数，否则下面的set指令不会执行
				set $image_file $3;
				set $image_type $4;
			}

			location /data {
				# 指定针对图片的日志格式，来分析图片类型和大小
				access_log logs/images.log mian;
				root /data/images;
				# 应用前面定义的变量。判断首先文件在不在，不在再判断目录在不在，如果还不在就跳转到最后一个url里
				try_files /$arg_file /image404.html;
			}

			location = /image404.html {
				# 图片不存在返回特定的信息
				return 404 "image not found\n";
			}
		}
	}

对形如 `/images/ef/uh7b3/test.png` 的请求，重写到 `/data?file=test.png`，于是匹配到 `location /data`，先看 `/data/images/test.png` 文件存不存在，如果存在则正常响应，如果不存在则重写 `tryfiles` 到新的 `image404 location`，直接返回404 状态码。

#### 例2：

	rewrite ^/images/(.*)_(\d+)x(\d+)\.(png|jpg|gif)$ /resizer/$1.$4?width=$2&height=$3? last;

对形如 `/images/bla_500x400.jpg` 的文件请求，重写到 `/resizer/bla.jpg?width=500&height=400` 地址，并会继续尝试匹配 `location`。