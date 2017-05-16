---
layout: post
title: linux远程开机(wake on lan)
date: 2017-05-16
categories: blog
tags: [linux]
description: linux远程开机(wake on lan)
---

### 一，什么情况下需要远程开机？

如果我们的服务器没有部署在本地（实际上通常都是这样的，我们会把服务器托管到IDC机房），
而且服务器在机房中不止一台，其中一台被关闭时，则我们可以远程连接一台没有关机的服务器上，然后进行远程开机。
  
### 二，远程开机需要的软件

它需要wakeonlan这个软件。

#### 从何处得到它？

它的官方站是：[http://sourceforge.net/projects/wake-on-lan/](http://sourceforge.net/projects/wake-on-lan/)

如果使用rpm包可以从这里下载：[http://dag.wieers.com/rpm/packages/wol/](http://dag.wieers.com/rpm/packages/wol/)

如果使用fedora，则可以用yum命令安装:

	yum install wol
    
### 三，如何进行远程开机？

先不要急着去关闭你的linux服务器，你首先要确定它是否支持远程开机？

#### 第一步：登录到目标服务器，用 ethtool 这个命令打印出网卡的信息

`[root@localhost lhd]# ethtool eth0`

	Settings for eth0:
	Supported ports: [ TP MII ]
	Supported link modes:   10baseT/Half 10baseT/Full
		100baseT/Half 100baseT/Full
	Supports auto-negotiation: Yes
	Advertised link modes:  10baseT/Half 10baseT/Full
		100baseT/Half 100baseT/Full
	Advertised auto-negotiation: Yes
	Speed: 100Mb/s
	Duplex: Full
	Port: MII
	PHYAD: 32
	Transceiver: internal
	Auto-negotiation: on
	Supports Wake-on: pumbg
	Wake-on: d
	Current message level: 0x00000007 (7)
	Link detected: yes

可以看到，ethtool把网卡的信息全部列出，我们只关心其中的这两项:

	Supports Wake-on: pumbg
	Wake-on: d  

如果 `wake-on` 一项值为 `d`,表示禁用 wake on lan。

值为 `g`,表示启用 wake on lan
                              
因为此机器禁用了 wake on lan,所以用下面的命令来启用它:

	[root@localhost lhd]# ethtool -s eth0 wol g

再用 ethtool命令进行查看,会发现:

	Wake-on: g

OK，目标机器的网卡已经支持了远程开机，下面我们得到它的本地MAC地址:

`[root@localhost lhd]# ifconfig`

	eth0      Link encap:Ethernet  HWaddr 00:03:0D:1D:1F:97
	inet addr:192.168.6.101  Mask:255.255.255.0
	UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
	RX packets:34470 errors:0 dropped:0 overruns:0 frame:0
	TX packets:35377 errors:0 dropped:0 overruns:0 carrier:0
	collisions:0 txqueuelen:1000
	RX bytes:31559763 (30.0 MiB)  TX bytes:5340032 (5.0 MiB)
	Interrupt:5 Base address:0x2c00

把 `HWaddr 00:03:0D:1D:1F:97` 这一项记录下来即可

现在你可以试着把目标机器关闭


#### 第二步：开机

现在我们需要登录到已安装了 wake on lan 软件的机器上，在上面执行开机命令:
      
	wol 00:03:0D:1D:1F:97

稍后就会发现，目标机器已开机可以登录了
     
### 四，多学一点:

#### 1、ethtool 的 `-s` 参数是修改指定以太网设备的设置

#### 2、wol 的取值默认是 `d` ,含义是 disable

修改后的值为 `g` ,含义是 Wake on MagicPacket(tm)
             
它还有几个取值，分别是： 
          
	p  Wake on phy activity
	u  Wake on unicast messages
	m  Wake on multicast messages
	b  Wake on broadcast messages
	a  Wake on ARP
                  
如果有兴趣，大家可以通过 `man ethtool` 查看
      
#### 3、当机器重启后，eth0 的设置又会回复到 `Wake-on: d` 状态，这个问题怎么解决？

两个办法：

##### 第一个，也是我们的惯性思维;

把 `/sbin/ethtool -s eth0 wol g` 这条命令附加到 `/etc/rc.local` 这个文件中，则下次开机后会自动执行

##### 第二个: 

编辑 `/etc/sysconfig/network-scripts/ifcfg-eth0`（eth0网卡的配置文件），添加上一行：

	ETHTOOL_OPTS=”wol g”

#### 4、网络唤醒的局限性：

它只能在局域网中应用，而不能通过互联网运行，为什么？

因为机器关闭后，完全是靠网卡唤醒机器，此时的机器是关闭的，没有操作系统运行，也就谈不上支持tcp/ip协议，当然也就不能通过互联网运行了。
        
也就是说：如果我们在某个局域网中只有一台机器，就不能使用此功能了。

我们必须能登录到局域网中的一台机器上，在此机器上运行 wake on lan 去唤醒目标机器

前提条件就是：**目标机器和我们登录的机器在同一局域网中**

#### 5、还有一点：

被远程开机的目标机器必须是插电的，没插电源的机器也能开机只有电影中才会出现。