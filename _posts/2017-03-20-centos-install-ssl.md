---
layout: post
title: CentOS服务器安装配置SSL
date: 2017-03-20
categories: blog
tags: [centos]
description: CentOS服务器安装配置SSL
---

https是一个安全的访问方式，数据在传输过程中是加密的，https基于ssl。
 
## 一、安装apache和ssl模块

### 1、安装apache
 
	#yum install httpd
 
### 2、安装ssl模块
 
	#yum install mod_ssl
 
重启apache：
 
	#service httpd restart
 
安装完mod_ssl会创建一个默认的SSL证书，路径位于`/etc/pki/tls`，此时可以立即通过https访问服务器了：
 
	https://Ｘ.Ｘ.Ｘ.Ｘ/
 
如果不使用默认的证书，也可以使用openssl手动创建证书。

## 二、使用openssl手动创建证书

### 1、安装openssl
 
	#yum install openssl
 
### 2、生成服务器私钥
 
	#cd /etc/pki/tls
	#openssl genrsa -out server.key 1024
 
注意：server.key 是私钥。

### 3、用私钥server.key文件生成证书请求文件csr
 
	#openssl req -new -key server.key -out server.csr
 
注：server.csr是证书请求文件。

此步骤需要输入一些证书信息：

	Country Name (2 letter code) [XX]:CN
	State or Province Name (full name) []:ZheJiang
	Locality Name (eg, city) [Default City]:WenZhou
	Organization Name (eg, company) [Default Company Ltd]:JiZhiNet
	Organizational Unit Name (eg, section) []:JiZhiNet
	Common Name (eg, your name or your server's hostname) []:Jzhi.com.cn
	Email Address []:IM@JZhi.Com.Cn
 
输入国家、省份、城市、公司、部门、姓名或服务器名、电子邮箱，随后会要求输入一个challengepassword（密码），无需输入，后面一律直接回车即可。 
 
### 4、生成数字签名crt文件（证书文件）
 
	#openssl x509 -days 365 -req -in server.csr -signkey server.key -out server.crt
 
用私钥签名证书请求文件，证书的申请机构和颁发机构都是自己。
 
### 5、编辑apache的ssl配置文件
 
	vi  /etc/httpd/conf.d/ssl.conf
 
/etc/httpd/conf.d/ssl.conf 文件配置具体如下：
 
	<VirtualHost _default_:443>
 
	DocumentRoot "/var/www/https"      //设置网页存放目录
	ServerName *:443                  //服务器的端口
	DirectoryIndex index.html index.html.var  //首页名称
 
	SSLEngine on
 
	SSLCertificateFile /etc/pki/tls/server.crt     //证书
	SSLCertificateKeyFile /etc/pki/tls/server.key   //私钥
 
	</VirtualHost>
 
### 6、重启apache

	#service httpd restart
 
访问 **https://ip/** ，就能看到证书信息了。

由于不是第三方根证书颁发机构颁发的证书，而是自己颁发的证书，所以浏览器会提示安全证书不受信任。
 
注意：**首页index.html 的文件权限为755**，否则将会出现如下提示：

	Forbidden
	Youdon't have permission to access /main.html on this server.

解决方法：修改首页index.html读写权限。

	#Chmod 755  index.html
 
### 关于openssl指令的补充说明：

	#openssl [操作]  -out  filename   [bits]
 
参数说明：
 
[操作]  主要的操作有如下两个：
 
	genrsa，建立RSA加密的Public key
	req，建立凭证要求文件或者是凭证文件
	-out ，后面加上输出的文件名，就是那把key name
	bits，用在genrsa加密的公钥的长度
	-x509，X.509，CertificateData  Management.   一种验证的管理方式

例：建立一支长度为1024bits的Public Key，注意文件名。

	#openssl genrsa  -out  Server.key 1024
 
生成证书请求命令：

	#Openssl req  -new  -key file.key  -out  file.csr -config  /path/to/openssl.cnf
 
	-config：指定openssl的配置文件路径，不指定时，默认会访问Unix格式的默认路径：/usr/local/ssl/openssl.cnf。

例：`#openssl req -new -key server.key -outserver.csr`
 
安装配置完成，启动httpd服务

	#service httpd start
 
输入服务器域名或者IP查看是否成功

1.如果不能打开请检查是否iptables没有开放80端口访问权限，可以先停止iptables服务，看是否这个原因造成。

	#service iptables stop
 
2.如果确定是iptables造成不能访问，编辑iptables来开放访问：

	#vim /etc/sysconfig/iptables
 
加入以下内容到iptables里面(如下图)：

	-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
 
重启iptables服务，看看是否配置正确

	#service iptables restart