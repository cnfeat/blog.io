---
layout: post
title: Ubuntu Server 11.10 安装 snmp
date: 2016-09-29
categories: blog
tags: [ubuntu]
description: Ubuntu Server 11.10 安装 snmp

---

操作系统：Ubuntu Server 11.10

### 1、安装配置snmp

`sudo apt-get install snmp snmpd   #安装，根据提示输入y即可`

`sudo mv /etc/snmp/snmpd.conf  /etc/snmp/snmpd.confbak  #备份原有配置文件`

`sudo vi /etc/snmp/snmpd.conf  #新建配置文件`

添加以下内容：

    com2sec notConfigUser default public
    group notConfigGroup v1 notConfigUser
    group notConfigGroup v2c notConfigUser
    view systemview included .1
    access notConfigGroup "" any noauth exact systemview none none
    syslocation localhost.com
    syscontact Root azrael@localhost.com
    pass .1.3.6.1.4.1.4413.4.1 /usr/bin/ucd5820stat

常用命令：

      /etc/init.d/snmpd start  #启动
      /etc/init.d/snmpd stop   #停止
      /etc/init.d/snmpd start  #重启


### 2、测试snmp

       netstat -nlup | grep ":161"  #检查snmp服务是否已在运行，出现类似下面的内容，说明snmp运行正常
       udp        0      0 0.0.0.0:161             0.0.0.0:*                           4999/snmpd
       lsof -i:161  #检查snmp端口是否监听，出现下面的内容，说明snmp端口监听正确
       snmpd   4999 snmp    7u  IPv4  17222      0t0  UDP *:snmp
       snmpwalk -v 2c -c public localhost  #有数据输出说明配置正确
       snmpwalk -v 2c -c public 192.168.21.168 #有数据输出说明配置正确

至此，Ubuntu Server 11.10安装snmp完成