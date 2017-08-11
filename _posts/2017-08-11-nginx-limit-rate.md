---
layout: post
title: nginx限制流量，限制用户并发连接数
date: 2017-08-11
categories: blog
tags: [nginx]
description: nginx限制流量，限制用户并发连接数
---

## Nginx限制流量

通过以下两条指令来完成限制流量。

### 指令名称：`limit_rate`

语法：`limit_rate speed`

默认值：`no`

使用环境：http，server，location，if in location

功能：该指令用于指定向客户端传输数据的速度，单位是每秒传输的字节数。该限制只是针对一个连接的设定。

### 指令名称：`limit_rate_after`

语法：`limit_rate_after size`

默认值：`limit_rate_after 1m`

使用环境：http，server，location，if in location中的 if 字段。

功能：可以理解为：在……后再限制速率为……。如 limit_rate_after 3m，解释为：以最大速度下载3MB后。

### 实例配置

     location /download {
          limit_rate_after 3m;
          limit_rate 512K;
     }

### 分析：

以最大下载速度下载3MB后，再以512K b/s的速度下载。

## Nginx 限制用户并发连接数

可以通过 `limit zone` 模块来达到限制用户的连接数的目的，即**限制同一用户IP地址的并发连接数。**

该模块提供了两个命令：`limit_zone` 和 `limit_conn`，其中，`limit_zone` 只能用在 http 区段，而 `limit_conn` 可以用在 http、server、location 区段。

### 实例配置

     http {
          limit_zone  one  $binary_remote_addr  10m;

          server {
               location  /download/ {
                    limit_conn  one 1;
               }
          }
     }

### 指令名称：`limit_zone`

语法：`limit_zone  zone_name  $variable  memory_max_size`

默认值：`no`

使用环境：http

功能：该指令用于定义一个 zone，该 zone 将会被用于存储会话的状态。能够存储的会话数量是由分配交付的变量和 memory_max_size 的大小决定的。

### 例如：

     limit_zone  one  $binary_remote_addr  10m;

### 分析：

客户端的 ip 地址被用作会话。注意这里使用的是 `$binary_remote_addr` 而不是 `$remote_addr`，

因为 `$remote_addr` 的长度为 `7~15 字节`，会话信息长度为 `32` 或`64 字节`；

`$binary_remote_addr` 的长度为 `4 字节`，会话信息长度为 `32 字节`。

因此，**二进制格式的 IP 地址比 ASCII 更高效。**

当设置 `1MB` 的一个 zone 时，如果使用 `$binary_remote_addr` 方式，该 zone 将会存放 `32000 个会话` 。（1MB/32=32768）

### 指令名称：limit_conn

语法：`limit_conn  zone_name  max_clients_per_ip`

默认值：`no`

使用环境：http，server，location

功能：该指令用于**为一个会话设定最大的并发连接数**。如果并发请求数超过这个限制，那么将会出现“Server unavailable”（503）错误。

### 例如：
	server {
		location  /download/ {
			limit_conn  one 1;
		}
	}
### 分析：

这个设置将会使来自同一IP的并发连接不能超过1个。

### 实例配置

	http {
		...
		lilmit_zone  flv_down  $binary_remote_addr  10m;
    
		server {
			...
			location /download {
				limit_conn  flv_down 1;
			}
		...
		}
	}

### 指令名称：`limit_conn_log_level`

语法：`limit_conn_log_level  info | notice | warn | error`

默认值：`error`

使用环境：http，server，location

功能：该指令用于设置日志的错误级别，当达到连接限制时，将会产生错误日志。