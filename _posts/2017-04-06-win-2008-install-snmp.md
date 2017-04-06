---
layout: post
title: Windows Server 2008 R2安装SNMP服务
date: 2017-04-06
categories: blog
tags: [windows]
description: Windows Server 2008 R2安装SNMP服务
---

### 说明：

> 在Windows Server 2003中SNMP服务是以Windows组件的形式来安装的，具体是通过：开始-设置-控制面板-添加或删除程序-添加删除Windows组件-管理和监视工具-简单网络管理协议（SNMP）来安装的。

在Windows Server 2008以及Windows Server 2008 R2中，SNMP是以一个服务器功能的形式存在的，安装方法也与Windows Server 2003完全不一样了，下面教大家在Windows Server 2008 R2中安装SNMP服务：

### 一、安装SNMP

1. 打开开始-管理工具-服务器管理

2. 服务器管理器-功能-添加功能

3. 选择SNMP服务，下一步

4. 点安装

5. 安装完成后，点关闭

重新启动服务器，使SNMP服务生效，否则不能正确配置SNMP

（注意：如果不想重启服务器，重新启动一下SNMP服务也可以）

#### 重启SNMP服务方法:

服务器管理器-配置-服务-SNMP Service 

鼠标右键-选择 “重新启动” 即可。

#### 配置SNMP服务

双击打开 SNMP Service，切换到安全选项，接受的社区名称-添加

	团体权限：只读
	社区名称：public

接受来自下列主机的SNMP数据包

编辑，SNMP 服务配置，主机名，IP或IPX 地址(H):

填写Cacti等监控主机的的IP地址: `192.168.1.100`

当然你也可以选择**“ 接受来自任何主机的SNMP数据包 ”**，为了安全，建议配置为监控主机的IP地址

最后，应用-确定，重启SNMP服务，使配置生效

### 二、设置防火墙，开启UDP161端口

远程监控主机是通过服务器 UDP161 端口来获取监控数据的，所以要设置防火墙对外开放 UDP161 端口

下面设置开启服务器的UDP161端口：

1. 打开防火墙：开始-控制面板-系统和安全-查看防火墙状态

2. 打开或关闭Windows防火墙

3. 家庭或工作（专用）网络位置设置：启用Windows防火墙

4. 公用网络位置设置：启用Windows防火墙

5. 最后确定

6. 打开高级设置

7. 点入站规则，再打开右上角的新建规则

8. 选择端口，下一步

9. 选择 `UDP`，特定本地端口：`161`，下一步

10. 允许连接，下一步

11. 何时应用该规则？域、专用、公用三个都勾选，下一步

12. 名称：SNMP	描述：SNMP

13. 点完成

防火墙设置完成。

### 三、安装SNMP增强插件，SNMP Informant-STD（可以监控到更多的服务器信息）

下载地址：[http://www.wtcs.org/informant/files/informant-std-16.zip](http://www.wtcs.org/informant/files/informant-std-16.zip)

下载好之后，双击安装，全部默认配置，安装完成之后，重启服务器

至此，Windows Server 2008 R2安装SNMP服务教程完成。