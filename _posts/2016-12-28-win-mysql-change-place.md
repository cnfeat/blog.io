---
layout: post
title: Windows下更改MySQL数据库的存储位置
date: 2016-12-28
categories: blog
tags: [mysql]
description: Windows下更改MySQL数据库的存储位置
---

在MySQL安装完成后，要修改数据库存储的位置，比如从安装目录下的 `C:\Program Files\MySQL\MySQL Server 5.0\Data` 文件夹转移到 `D:\mySQLData` 文件夹，方法如下：

#### 1、在 `D:\` 下新建 mySQLData 文件夹

#### 2、停止MySQL服务，将 `C:\Program Files\MySQL\MySQL Server 5.0\Data` 下的文件夹和文件一起拷贝到 `D:\mySQLData` 文件夹下

#### 3、在安装目录下找到 `my.ini` 文件，打开找到：

    #Path to the database root
    datadir="C:/Program Files/MySQL/MySQL Server 5.0/Data/"

 
将 `datadir` 的值更改为 `D:/mySQLData/`

保存后，重新启动mySQL服务即可。


----------

 
### 错误处理：

如果报1067错误，可以打开 `my.ini` 将 `datadir` 的值更改为 `D:/mySQLData/` 先直接重启，
重启成功后再把原来老的数据文件都拷贝过来
 
注意：完成之后不要删除C:/ProgramData/MySQL/MySQL Server 5.1/data/目录（尽管可以删除），因为
以后重新配置时删除现有实例，再配置新的实例时可能会出现服务无法启动的问题，因为程序还会默认的
把C:/ProgramData/MySQL/MySQL Server 5.1/data/作为数据库目录。。。