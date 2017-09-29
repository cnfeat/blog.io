---
layout: post
title: Linux下Nginx日志分析
date: 2017-05-25
categories: blog
tags: [linux,nginx]
description: Linux下Nginx日志分析
---

## Access logs 日志格式

以nginx默认的日志格式为例：

	$remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_user_agent"

### 各字段的含义分别是：

	$remote_addr 	请求者IP
	$remote_user 	HTTP授权用户，如果不使用Http-based认证方式，其值为空
	[$time_local] 	服务器时间戳
	"$request" 		HTTP请求类型(如GET，POST等)+HTTP请求路径(不含参数)+HTTP协议版本
	$status 		服务器返回的状态码(如200，404，5xx等)
	$body_bytes_sent 	服务器响应报文大小，单位byte
	"$http_referer" 	referer字段值
	"$http_user_agent" 	User Agent字段

## 常用的日志分析命令

### 根据状态码进行请求次数排序

	cat access.log | cut -d '"' -f3 | cut -d ' ' -f2 | sort | uniq -c | sort -r

### 或者使用awk:

	awk '{print $9}' access.log | sort | uniq -c | sort -r

上例显示有704次404请求

### 接下来是如何找到这些请求的URL

	awk '($9 ~ /404/)' access.log | awk '{print $7}' | sort | uniq -c | sort -r

输出样列：

	21 /members/katrinakp/activity/2338/19 /blogger-to-wordpress/robots.txt
	14 /rtpanel/robots.txt

### 接下来考虑如果找到这些请求的IP地址，使用命令：

	awk -F\" '($2 ~ "/wp-admin/install.php"){print $1}' access.log | awk '{print $1}' | sort | uniq -c | sort -r

输出样例：

	14 50.133.11.248
	12 97.106.26.244
	11 108.247.254.37
	10 173.22.165.123

### php后缀的404请求（通常是嗅探）

	awk '($9 ~ /404/)' access.log | awk -F\" '($2 ~ "^GET .*\.php")' | awk '{print $7}' | sort | uniq -c | sort -r | head -n 20

### 按URL的请求数排序

	awk -F\" '{print $2}' access.log | awk '{print $2}' | sort | uniq -c | sort -r

### url包含XYZ:

	awk -F\" '($2 ~ "ref"){print $2}' access.log | awk '{print $2}' | sort | uniq -c | sort -r