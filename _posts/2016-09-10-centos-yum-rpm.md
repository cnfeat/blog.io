---
layout: post
title: centos 的软件安装方法 rpm 和 yum
date: 2016-09-10
categories: blog
tags: [centos,nginx]
description: centos 的软件安装方法 rpm 和 yum

---

## centos的软件安装大致可以分为两种类型：

1. rpm 文件安装，使用 rpm 指令  类似 [ubuntu]deb 文件安装，使用 dpkg 指令

2. yum 安装   类似 [ubuntu]apt-get 安装

## rpm命令

### (一)查询系统装已经安装的软件信息

对于一个rpm包来说，都是有"-"和"."构成的，

基本上有以下几部分组成： * 包名 * 版本信息 * 发布版本号 * 运行平台，

当出现noarch,代表的是软件可以平台兼容

#### 1）查询系统中已经安装的软件

`rpm -qa `

#### 2）查询一个已经安装的文件属于哪个软件包；

`rpm -qf 文件名的绝对路径`

#### 3）查询已安装软件包都安装到何处；

软件名定义是:rpm包去除平台信息和后缀后的信息

`rpm -ql 软件名`

#### 4）查询一个已安装软件包的信息

`rpm  -qi 软件名`

#### 5）查看一下已安装软件的配置文件；

`rpm -qc 软件名`

#### 6）查看一个已经安装软件的文档安装位置：

`rpm -qd 软件名`

#### 7）查看一下已安装软件所依赖的软件包及文件；

`rpm -qR 软件名`
 
### (二)对于未安装的软件包信息查询

#### 1）查看一个软件包的用途、版本等信息；

`rpm -qpi rpm文件`

#### 2）查看一件软件包所包含的文件；

`rpm -qpl rpm文件`

#### 3）查看软件包的文档所在的位置；

`rpm -qpd rpm文件`

#### 4）查看一个软件包的配置文件；

`rpm -qpc rpm文件`

#### 5）查看一个软件包的依赖关系

`rpm -qpR rpm文件`
 
### (三)软件包的安装、升级、删除等

#### 1)安装或者升级一个rpm包

`rpm -ivh rpm文件【安装】`
`rpm -Uvh rpm文件【更新】`

#### 2)删除一个rpm 包

`rpm -e 软件名`

如何需要不管依赖问题，强制删除软件，

在如上命令其后加上 --nodeps
 
### (四)签名导入

`rpm --import 签名文件`

`rpm --import RPM-GPG-KEY`

## yum命令

### (一)yum基本概念

#### 1)yum是什么

yum = Yellow dog Updater, Modified  

主要功能是更方便的添加/删除/更新RPM包.  

它能自动解决包的倚赖性问题.  

它能便于管理大量系统的更新问题

#### 2)yum的特点

- 可以同时配置多个资源库(Repository)  
- 简洁的配置文件(/etc/yum.conf  
- 自动解决增加或删除rpm包时遇到的倚赖性问题  
- 使用方便 
- 保持与RPM数据库的一致性

#### 3)yum安装

CentOS自带(yum-*.noarch.rpm) 

`#rpm -ivh yum-*.noarch.rpm `

在第一次启用 yum 之前首先需要导入系统的 RPM-GPG-KEY：
 
### (二)yum指令的使用

当第一次使用 yum 管理软件时，yum 会自动下载所需要的 headers 放置在 /var/cache/yum 目录下；

#### 1) rpm包的更新

##### 检查可以更新的软件包 

`yum check-update` 

##### 更新所有的软件包 

`yum update` 

##### 更新特定的软件包 

`yum update kernel` 

##### 大规模的升级 

`yum upgrade`

#### 2) rpm包的安装和删除

##### rpm包的安装和删除 

`yum install xxx【服务名】` 

`yum remove xxx【服务名】`

#### 3) yum缓存的相关信息

##### 清楚缓存中rpm包文件 

`yum clean packages`
 
##### 清楚缓存中rpm的头文件
 
`yum clean  headers`
 
##### 清除缓存中旧的头文件
 
`yum clean old headers`
 
##### 清除缓存中旧的rpm头文件和包文件 

`yum clean all`

#### 4)软件包信息查询

##### 列出资源库中所有可以安装或更新的rpm包 

`yum list` 

##### 列出资源库中特定的可以安装或更新以及已经安装的rpm包
 
`yum list firfox*` 

N:可以在rpm包名中使用通配符,查询类似的rpm包

##### 列出资源库中所有可以更新的rpm包 

`yum list updates` 

##### 列出已经安装的所有的rpm包 

`yum list installed` 

##### 列出已经安装的但是不包含在资源库中的rpm包 

`yum list extras`
 
N:通过如网站下载安装的rpm包  

##### rpm包信息显示(info参数同list)，列出资源库中所有可以安装或更新的rpm包的信息 

`yum info` 

##### 列出资源库中特定的可以安装或更新以及已经安装的rpm包的信息 

`yum info firefox*` 

N:可以在rpm包名中使用匹配符 

##### 列出资源库中所有可以更新的rpm包的信息

`yum info updates` 

##### 列出已经安装的所有的rpm包的信息

 `yum info installed` 

##### 列出已经安装的但是不包含在资源库中的rpm包的信息 

`yum info extras` 

N:通过如网站下载安装的rpm包的信息

##### 搜索匹配特定字符的rpm包

`yum search firofox`

##### 搜索包含特定文件的rpm包

`yum provides firefox`
 
### (三)yum软件源更新

[http://mirrors.163.com/.help/centos.html](http://mirrors.163.com/.help/centos.html)