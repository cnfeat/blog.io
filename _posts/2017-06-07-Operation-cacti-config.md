---
layout: post
title: CactiEZ 中文版 V10.1 配置安装方法
date: 2017-06-07
categories: blog
tags: [operations]
description: CactiEZ 中文版 V10.1 配置安装方法
---

> 说明：CactiEZ中文版V10.1是基于CentOS 6.0系统，整合Cacti等相关软件，重新编译而成的一个操作系统！
> 
> 优点：省去了复杂烦琐的Cacti配置过程，安装之后即可使用，全部中文化，界面更友好
> 
> 缺点：CactiEZ是一个完整的操作系统，需要专门一台电脑才能安装使用

### 具体案例：

#### 1、CactiEZ监控主机

	IP：192.168.21.175

	子网掩码：255.255.255.0

	网关：192.168.21.2

	DNS：8.8.8.8		8.8.4.4

#### 2、Windows客户机

系统：Windows Server 2003

IP：192.168.21.130，与CactiEZ监控主机在同一个局域网内

#### 3、Linux客户机

系统：CentOS 6.2

IP：192.168.21.169，与CactiEZ监控主机在同一个局域网内

### 目标：使用CactiEZ监控主机对Windows客户机和Linux客户机进行监控

## 一、安装CactiEZ监控主机

CactiEZ下载地址（下载方法：复制下载地址到迅雷等下载工具里面进行下载）：

32位: [http://www.zhengfeng.net/CactiEZ-10.1-i386.torrent](http://www.zhengfeng.net/CactiEZ-10.1-i386.torrent)

64位: [http://www.zhengfeng.net/CactiEZ-10.1-x86_64.torrent](http://www.zhengfeng.net/CactiEZ-10.1-x86_64.torrent)

下面以安装32为CactiEZ系统为例，64位系统安装方法相同

**特别说明：安装CactiEZ的主机磁盘空间必须要在10G以上，否则不能安装。如果是虚拟机安装，请设置磁盘空间大于10G**

把下载好的CactiEZ系统镜像刻录为光盘（或做成启动U盘），使用光盘成功引导系统之后，会出现引导菜单界面

选择第一项，安装CactiEZ，回车

检查安装介质，这里选择Skip直接跳过，回车，系统会自动安装

系统已经安装完成，点击Reboot重启系统！

## 二、设置CactiEZ监控主机

默认安装好之后，**系统登录用户root，密码CactiEZ**

以下操作在登录系统之后进行

### 1、修改root登录密码

passwd root #回车之后，提示输入2次新密码

出现：

`passwd：all authentication tokens updated successfully.`

说明密码修改成功

### 2、修改IP地址、子网掩码、网关、DNS等信息

`vi /etc/sysconfig/network-scripts/ifcfg-eth0`

	DEVICE="eth0"
	BOOTPROTO="static"
	DNS1="8.8.8.8"
	DNS2="8.8.4.4"
	GATEWAY="192.168.21.2"
	HOSTNAME="CactiEZ.local"
	HWADDR="00:0C:29:AF:98:C1"
	IPADDR="192.168.21.175"
	MTU="1500"
	NETMASK="255.255.255.0"
	NM_CONTROLLED="yes"
	ONBOOT="yes"

`:wq! #保存`

`service network restart #重启网络`

### 3、登录CactiEZ监控平台

浏览器地址栏里面输入，Cacti监控主机的IP地址，回车

**用户名：admin，默认密码：admin**

为了安全考虑，第一次登录之后必须修改默认密码，修改好之后点保存，登录到CactiEZ Web监控平台

至此，CactiEZ监控主机安装完成。

## 二、配置被监控的Windows客户机

**说明：要使用CactiEZ监控一台Windows主机，需要在被监控的主机上面安装snmp（简单网络管理协议）**

### 1、下面开始安装配置snmp

开始-设置-控制面板-添加或删除程序-添加删除Windows组件

找到`管理和监视工具`，前面打勾，在点`详细信息`：`简单网络管理协议（SNMP）`前面打勾，然后却确定，下一步

开始安装，直到安装完成（注意：安装过程中会提示插入Windows系统光盘，这个需要提前准备好。）

### 2、配置SNMP服务

开始-运行，输入 `services.msc` 确定，打开服务管理

找到 `SNMP Service` 选项，双击打开，切换到`安全`选项，在接受团体名称下面点击`添加`，出现SNMP服务配置

	团体权限：只读
	团体名称：public

最后确定

选中：`接受来自这些主机的SNMP数据包`，

点下面的`添加`，在主机名，IP或IPX地址中输入`CactiEZ监控主机的IP`

这里输入：`192.168.21.175`，最后，`应用`，`确定`。

最后重启Windows系统

## 三、配置被监控的Linux客户机

**说明：要使用CactiEZ监控一台Linux主机，需要在被监控的主机上安装net-snmp等相关的软件包；
同时需要开启防火墙UDP161端口。**

### 1、开启防火墙UDP161端口

`vi /etc/sysconfig/iptables #编辑防火墙配置`

	-A INPUT -m state --state NEW -m udp -p udp --dport 161 -j ACCEPT

`/etc/init.d/iptables restart #重启防火墙使配置生效`

### 2、安装net-snmp（这里使用CentOS的yum命令在线安装）

	yum -y install net-snmp

	chkconfig snmpd on #设置开机启动

	service snmpd start #启动snmpd

### 3、配置snmp

`cp /etc/snmp/snmpd.conf /etc/snmp/snmpd.confbak #备份配置文件`

`vi snmpd.conf #添加下面代码`

	com2sec notConfigUser default public
	group notConfigGroup v1 notConfigUser
	group notConfigGroup v2c notConfigUser
	view systemview included .1
	access notConfigGroup "" any noauth exact systemview none none
	syslocation www.osyunwei.com
	pass .1.3.6.1.4.1.4413.4.1 /usr/bin/ucd5820stat

`:wq! #保存退出`

`service snmpd restart #重启snmp`

`netstat -nlup |grep ":161" #检查snmp服务器是否运行`

出现下面输出结果，说明snmp运行正常

	udp 0 0 0.0.0.0:161 0.0.0.0:* 3265/snmpd

## 四、设置CactiEZ对Windows和Linux进行监控

以下操作在登录 `http://192.168.21.175` 之后进行

### 1、添加对Windows主机的监控

`管理`-`主机`，点`添加`

相关选项都有具体中文说明：

特别注意：

	主机名：要监控的主机的IP地址，这里是192.168.21.130
	主机模板：选择Windows主机
	监视主机：后面打勾，表示启用
	SNMP团体名称：务必要与Windows主机之前设置的SNMP团体名称相同，否则监控失败，这里是public
	SNMP端口：默认是161

其他选项默认即可

最后，点`保存`，会出现下个界面

点击`为这个主机添加图形`，根据自己需要监控的对象选中右边的复选框，点添加

注意，最后一项，选择一个图形类型，**32位主机选择流入/流出 位** ；**64位主机选择流入/流出 （64位）**

出现下个界面，再点`添加`

点上面导航栏的`监视器`，进入监视界面

点击打开我们刚才添加的主机，已经可以看到监控的图形了，只是这个时候还没有数据，数据采集是1分钟轮询一次

等待几分钟之后，刷新，会看到下面的界面，这个时候已经有了监控数据了。

Windows 主机监控设置完成

### 2、添加对Linux主机的监控

添加方法与Windows相同

**注意：主机名字填写Linux主机的IP，主机模板选择Linux主机**

最后点`添加`，再点 `为这个主机添加图形`，根据自己需要监控的对象选中右边的复选框，点`添加`

出现下个界面，再点`添加`

然后点上面导航栏的`监视器`，进入监视界面，选择刚才添加的Linux主机

同样等待几分钟之后，会看到如下的监控数据

Linux 主机监控设置完成

至此，CactiEZ 中文版V10.1配置安装设置完成。