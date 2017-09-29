---
layout: post
title: Nginx安装SSL证书
date: 2017-03-18
categories: blog
tags: [nginx]
description: Nginx安装SSL证书
---

### 第一步：获取证书

1. 通过网站，下载到SSL证书

2. 把SSL证书文件（包括“-----BEGIN CERTIFICATE-----”和“-----END CERTIFICATE-----”）粘贴到记事本TXT文本编辑器中，在SSL证书文件代码文本结尾处回车换行

3. 再把CA证书文件代码复制粘贴到SSL证书文件下面（包 括“-----BEGIN CERTIFICATE-----”和“-----END CERTIFICATE-----”）

4. 修改证书文件扩展名，保存包含SSL证书文件代码与CA证书文件代码的文本文件为 **yourdomain_server.crt** 文件。

### 第二步：修改配置文件

最后把 **yourdomain_server.crt 、yourdomain_server.key** (生成csr时生成的私钥)两个文件保存到同一个目录，例如 `/usr/local/nginx/conf` 目录下。 

用文本编辑器打开Nginx根目录下 `conf/nginx.conf` 文件找到并更新一下内容：

	server { 
		listen       443 ssl;         
		server_name  www.yourdomain.com;   
      
		ssl_certificate      yourdomain_server.crt;        
		ssl_certificate_key  yourdomain_server.key;     
   
		ssl_session_timeout  5m;    
     
		ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;         
		ssl_ciphers  HIGH:!aNULL:!MD5;   
           
		location / {             
			root   html;            
			index  index.html index.htm;        
		}    
	} 

#### 配置文件参数说明：  

	listen       443 ssl     启用SSL功能并设置端口号为443 
	ssl_certificate          证书文件yourdomain_server.crt 
	ssl_certificate_key      私钥文件yourdomain_server.key 

按照以上的步骤配置完成后，重新启动 nginx。

	service nginx restart

#### Nginx官方HTTPS参数配置说明 

	Nginx版本                              官方默认ssl_protocols参数 
	Version 1.9.1 和之后                   TLSv1 TLSv1.1 TLSv1.2 
	Version 0.7.65, 0.8.19 和之后          SSLv3 TLSv1 TLSv1.1 TLSv1.2 
	Version 0.7.64, 0.8.18 和之前          SSLv2 SSLv3 TLSv1   

	Nginx版本                              官方默认ssl_ciphers参数 
	Version 1.0.5 和之后                   HIGH:!aNULL:!MD5 
	Version 0.7.65, 0.8.20 和之后          HIGH:!ADH:!MD5 
	Version 0.8.19                        ALL:!ADH:RC4+RSA:+HIGH:+MEDIUM 
	Version 0.7.64, 0.8.18 和之前          ALL:!ADH:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP 

Nginx官方HTTPS配置文档： [http://nginx.org/en/docs/http/configuring_https_servers.html](http://nginx.org/en/docs/http/configuring_https_servers.html)