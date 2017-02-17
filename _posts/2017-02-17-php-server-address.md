---
layout: post
title: 关于PHP获取服务器地址的方法
date: 2017-02-17
categories: blog
tags: [php]
description: 关于PHP获取服务器地址的方法
---

### 1、获取服务器本地地址，最先想到的方法是根据 `$_SERVER['SERVER_ADDR']`。

但在命令行运行程序时，获取就获取不到结果。

	<?php 
		var_dump($_SERVER['SERVER_ADDR']);
 

网页访问结果：`string(14) "172.16.152.239"`

命令行结果：

	[root@lamp1 www]# php index.php 
	NULL

### 2、使用 gethostbyname 获取

对于命令行来说，可以取到 `$_SERVER['HOSTNAME']`，

如果在 `/etc/hosts` 里设置了本机名称对应的ip地址的话，则可以使用 `gethostbyname($_SERVER['HOSTNAME'])` 来获取服务器IP地址，再结合 `$_SERVER['SERVER_ADDR']` 就得到通用的方法


	<?php
		function get_server_ip(){
        	if(!empty($_SERVER['SERVER_ADDR']))
                return $_SERVER['SERVER_ADDR'];
        	return gethostbyname($_SERVER['HOSTNAME']);
	}

	var_dump(get_server_ip());
 

缺点也很明显：就是如果机器没有设置 `hosts` 的话 `gethostbyname` 就解析不出 ip 地址，只能获取本机名称

### 3、使用系统命令

最原始也是最奏效的方法，使用底层信息 `ifconfig(ipconfig for windows)` 。使用 `shell_exec` 命令执行 `ifconfig`，然后从字符串中解析出地址来


	<?php
		function get_server_ip(){
        	if(!empty($_SERVER['SERVER_ADDR']))
                return $_SERVER['SERVER_ADDR'];
        	$result = shell_exec("/sbin/ifconfig");
        	if(preg_match_all("/addr:(\d+\.\d+\.\d+\.\d+)/", $result, $match) !== 0){
                	foreach($match[0] as $k=>$v){
                        if($match[1][$k] != "127.0.0.1")
                                return $match[1][$k];
                	}
        	}
        return false;
	}


### 自己用在服务器上的代码（用花生壳申请了一个域名，绑定家中宽带的动态IP）：

	<!-- show my local server IP -->
	<?php
		function get_server_ip(){
			return gethostbyname("XXX.imwork.net");
		}
		echo(get_server_ip());
	?>
	<!-- show IP end -->
