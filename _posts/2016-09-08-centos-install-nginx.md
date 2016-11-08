---
layout: post
title: CentOS 6.4 快速安装 Nginx
date: 2016-09-08
categories: blog
tags: [centos,nginx]
description: CentOS 6.4 快速安装 Nginx

---

## 安装Nginx

### (1)下载Nginx的RPM包

(http://nginx.org/en/download.html)

`wget http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm`

### (2)安装rpm包

`rpm -ivh nginx-release-centos-6-0.el6.ngx.noarch.rpm`

或直接：

`rpm -ivh http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm`

### (3)采用yum进行安装Nginx

`yum install nginx`

注意：需要按y进行确认安装

### 修改防火墙配置

`vi /etc/sysconfig/iptables`

加入规则

### 重启防火墙

`service iptables restart`

### 启动 Nginx

`nginx`

打开IE测试