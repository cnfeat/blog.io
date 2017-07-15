---
layout: post
title: Nginx安全配置，防止网站被攻击
date: 2017-07-15
categories: blog
tags: [nginx]
description: Nginx安全配置，防止网站被攻击
---

> 网站被攻击是一个永恒不变的话题，网站攻击的方式也是一个永恒不变的老套路。找几百个电脑（肉鸡），控制这些电脑同时访问你的网站，超过你网站的最大承载能力，然后你就瘫了。方法虽然老土，但却一直都很管用。
> 
> 网站其实只要控制能够进入的访问数量，你就攻击不了我。


Nginx 有 2 个模块用于控制访问“数量”和“速度”，简单的说，控制你**最多同时有多少个访问**，并且控制你**每秒钟最多访问多少次**， 你的同时并发访问不能太多，也不能太快，不然就“杀无赦”。

	HttpLimitZoneModule    限制同时并发访问的数量
	HttpLimitReqModule     限制访问数据，每秒内最多几个请求

请先检查你的 nginx 是否有这 2 个模块。
 
## 1、普通配置

啥叫普通配置 ？ 就是说，你的服务器直接面向普通用户，例如：

**普通用户IE浏览器  ——->  你的服务器**

由于普通用户直接访问你的服务器，所以你可以直接得到用户的 IP 地址， 而我们的限制就是基于对 来源IP 地址的访问限制

Nginx 里面设置一个限制

	## 用户的 IP 地址 $binary_remote_addr 作为 Key，每个 IP 地址最多有 50 个并发连接
	## 你想开 几千个连接 刷死我？ 超过 50 个连接，直接返回 503 错误给你，根本不处理你的请求了
	limit_conn_zone $binary_remote_addr zone=TotalConnLimitZone:10m ;
	limit_conn  TotalConnLimitZone  50;
	limit_conn_log_level notice;
 
	## 用户的 IP 地址 $binary_remote_addr 作为 Key，每个 IP 地址每秒处理 10 个请求
	## 你想用程序每秒几百次的刷我，没戏，再快了就不处理了，直接返回 503 错误给你
	limit_req_zone $binary_remote_addr zone=ConnLimitZone:10m  rate=10r/s;
	limit_req_log_level notice;
 
	## 具体服务器配置
	server {
	    listen   80;
	    location ~ \.php$ {
			## 最多 5 个排队， 由于每秒处理 10 个请求 + 5个排队，你一秒最多发送 15 个请求过来，再多就直接返回 503 错误给你了
			limit_req zone=ConnLimitZone burst=5 nodelay;
 
			fastcgi_pass   127.0.0.1:9000;
			fastcgi_index  index.php;
			include    fastcgi_params;
	    }    
 
	}

这样一个最简单的服务器安全限制访问就完成了，这个基本上你 Google 一搜索能搜索到  90% 的网站都是这个例子，而且还强调用“$binary_remote_addr”可以节省内存之类的废话
 
## 2、带有 CDN 加速网站的 Nginx 安全配置

很多时候，我们的网站不是简单的

**普通用户IE浏览器  ——->  你的服务器 ** 

的结构， 考虑到网络访问速度问题，我们中间可能会有各种 网络加速（CDN）。

考虑到网站的安全性和访问加速，我们的架构是：

**普通用户浏览器  —–>  360网站卫士加速（CDN，360防 CC,DOS攻击） ——>  阿里云加速服务器（我们自己建的CDN，阿里云盾） —-> 源服务器（PHP 程序部署在这里，iptables, nginx 安全配置）**

可以看到，我们的网站中间经历了好几层的透明加速和安全过滤， 这种情况下，我们就不能用上面的“普通配置”。

因为上面基于  源IP的限制 结果就是，我们把 360网站卫士  或者  阿里云盾 给限制了，因为这里“源IP”地址不再是  普通用户的IP，而是中间  网络加速服务器 的IP地址。

我们需要限制的是 最前面的普通用户，而不是中间为我们做加速的 加速服务器。
 
### 2.1 现在我们面对的最直接的问题就是， 经过这么多层加速，我怎么得到“最前面普通用户的 IP 地址”呢？

(这里只说明结果，不了解 Http 协议的人请自行 Google 或者 Wikipedia  [http://zh.wikipedia.org/zh-cn/X-Forwarded-For](http://zh.wikipedia.org/zh-cn/X-Forwarded-For)  )

当一个 CDN 或者透明代理服务器把用户的请求转到后面服务器的时候，这个 CDN 服务器会在 Http 的头中加入 一个记录

	X-Forwarded-For :  用户IP, 代理服务器IP

如果中间经历了不止一个代理服务器，中间建立多层代理之后，这个 记录会是这样

	X-Forwarded-For :  用户IP, 代理服务器1-IP, 代理服务器2-IP, 代理服务器3-IP, ….

可以看到经过好多层代理之后， 用户的真实IP 在第一个位置， 后面会跟一串 中间代理服务器的IP地址，从这里取到用户真实的IP地址，针对这个 IP 地址做限制就可以了，
 
### 2.2 经过多层CDN之后取得原始用户的IP地址，nginx 配置

取得用户的原始地址

	map $http_x_forwarded_for  $clientRealIp {
			## 没有通过代理，直接用 remote_addr
	    ""    $remote_addr;  
			## 用正则匹配，从 x_forwarded_for 中取得用户的原始IP
			## 例如   X-Forwarded-For: 202.123.123.11, 208.22.22.234, 192.168.2.100,...
			## 这里第一个 202.123.123.11 是用户的真实 IP，后面其它都是经过的 CDN 服务器
	    ~^(?P<firstAddr>[0-9\.]+),?.*$    $firstAddr;
	}
 
	## 通过 map 指令，我们为 nginx 创建了一个变量 $clientRealIp ，这个就是原始用户的真实 IP 地址，
	## 不论用户是直接访问，还是通过一串 CDN 之后的访问，我们都能取得正确的原始IP地址
 
### 2.3 测试、测试

很多时候，你在网上搜到一堆配置，你照着做了，但是你怎么知道这个配置真的正确 ？是的，我们需要自己做一个有效的真实的测试，验证它是正确的之后才真的采用它

Nginx 这种配置怎么测试呢？ 用 Echo 模块，如果你知道 Nginx 这个模块的话。

我们首先测试这个 `$clientRealIp` 是否真的是我们客户机的 IP 地址，在网站上增加一个访问地址，比如  `www.test.com/nginx-test`，配置如下：

给 Nginx 增加一个测试地址

	server {
    	listen   80;
        	server_name  www.test.com;
 
        	## 当用户访问 /nginx-test 的时候，我们输出 $clientRealIp 变量，看看这个变量值是不是真的 用户源IP 地址
        	location /nginx-test {
                	echo $clientRealIp;
        	}
	}

接下来，用你的浏览器访问  `www.test.com/nginx-test`，这个时候会弹出框下载一个文件 nginx-test，下载完成用 notepad++ 打开，里面就是一个 IP 地址

访问 `www.ip138.com` ，看看这个里面记录的IP地址是否和 ip138 侦测的IP 一致？

通过这种方式，你就可以对 Nginx 的一些复杂配置做有效的测试。

经过测试，我们确认 通过多层CDN 之后，$clientRealIp 仍然是有效的原始用户IP地址
 
### 2.4 根据用户的真实 IP 做连接限制

下面是修改之后的 Nginx 配置：

CDN环境下 Nginx 的安全配置

	## 这里取得原始用户的IP地址
	map $http_x_forwarded_for  $clientRealIp {
    	""    $remote_addr;
    	~^(?P<firstAddr>[0-9\.]+),?.*$    $firstAddr;
	}
 
	## 针对原始用户 IP 地址做限制
	limit_conn_zone $clientRealIp zone=TotalConnLimitZone:20m ;
	limit_conn  TotalConnLimitZone  50;
	limit_conn_log_level notice;
 
	## 针对原始用户 IP 地址做限制
	limit_req_zone $clientRealIp zone=ConnLimitZone:20m  rate=10r/s;
	#limit_req zone=ConnLimitZone burst=10 nodelay;
	limit_req_log_level notice;
 
	## 具体服务器配置
	server {
		listen   80;
		location ~ \.php$ {
			## 最多 5 个排队， 由于每秒处理 10 个请求 + 5个排队，你一秒最多发送 15 个请求过来，再多就直接返回 503 错误给你了
			limit_req zone=ConnLimitZone burst=5 nodelay;
 
			fastcgi_pass   127.0.0.1:9000;
			fastcgi_index  index.php;
			include    fastcgi_params;
		}    
	}
 
## 后记：

通过上面的配置，现在你的网站可以完美的配合任何 网络加速服务（CDN）的使用，并且同时能保证对“最终用户的限制”。
