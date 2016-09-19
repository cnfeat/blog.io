---
layout: post
title: centos 下搭建 SVN 服务器
date: 2016-09-19
categories: blog
tags: [centos]
description: centos下搭建 SVN 服务器

---

## 安装步骤如下：

### 1、使用 yum 在线安装软件包

`yum install subversion`

### 2、查看安装位置

`rpm -ql subversion`

我们知道svn在bin目录下生成了几个二进制文件。

#### 查看svn的使用方法

`svn --help`
 
### 3、创建svn版本库目录

`mkdir -p /var/svn/svnrepos`
 
### 4、创建版本库

`svnadmin create /var/svn/svnrepos`

执行了这个命令之后会在 **/var/svn/svnrepos** 目录下生成如下这些文件

 
### 5、进入conf目录（该svn版本库配置文件）

authz 文件是权限控制文件

passwd 是帐号密码文件

svnserve.conf SVN 服务配置文件
 
### 6、设置帐号密码

`vi passwd`

在 [users] 块中添加用户和密码

格式：**帐号=密码**，如 dan=dan
 
### 7、设置权限

`vi authz`

在末尾添加如下代码：

    [/]
    dan=rw

意思是：版本库的 根目录 dan 对其有 读写 权限。
 
### 8、修改svnserve.conf文件

`vi svnserve.conf`

打开下面的几个注释：

    anon-access = read #匿名用户可读
    auth-access = write #授权用户可写
    password-db = passwd #使用哪个文件作为账号文件
    authz-db = authz #使用哪个文件作为权限文件
    realm = /var/svn/svnrepos # 认证空间名，版本库所在目录
 
### 9、启动svn版本库

`svnserve -d -r /var/svn/svnrepos`
 
### 10、在windows上测试 ( 要事先安装TortoiseSVN )

新建一个测试文件夹

在该文件夹下右键选择 SVN checkout

填写SVN的地址
 
输入密码

操作完成。

## 常用命令总结：
 
#### 启动 SVN

`[root@test2 conf]# svnserve -d -r /var/svn/svnrepos`
 
#### 查看SVN进程

`[root@test2 conf]# ps -ef|grep svn|grep -v grep`

root      2301     1  0 18:58 ?        00:00:00 svnserve -d -r  /var/svn/svnrepos
 
#### 检测SVN端口

`[root@test2 conf]# netstat -ln |grep 3690`

tcp        0      0 0.0.0.0:3690                0.0.0.0:*                   LISTEN     
 
#### 停止 SVN 服务

`[root@test2 conf]# killall svnserve   //停止`