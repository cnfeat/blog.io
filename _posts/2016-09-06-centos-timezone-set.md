---
layout: post
title: 设置修改CentOS系统时区/时间
date: 2016-09-06
categories: blog
tags: [centos]
description: 设置修改CentOS系统时区/时间

---

## centos时间调整的操作

如果没有安装，而你使用的是 CentOS系统 那使用命令

`yum install ntp`

然后：

`ntpdate us.pool.ntp.org`
 
时区是以文件形式存在的

当前的时区文件是在 **/etc/localtime**

那么其他时区的文件在 **/usr/share/zoneinfo** 下

我们用东八区，北京，上海的时间

    #cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    #reboot
 
### 一、时区

#### 查看当前时区

`date -R`

复制相应的时区文件，替换CentOS系统时区文件；或者创建链接文件

`cp /usr/share/zoneinfo/$主时区/$次时区 /etc/localtime`

#### 在中国可以使用：

`cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`
 
### 二、时间

#### 1、查看时间和日期

`date`

#### 2、设置时间和日期

将CentOS系统日期设定成1996年6月10日的命令

`date -s 06/10/96`

将CentOS系统时间设定成下午1点52分0秒的命令

`date -s 13:52:00`

#### 3. 将当前时间和日期写入BIOS，避免重启后失效

`hwclock -w`
 
### 三、定时同步时间

`# /usr/sbin/ntpdate us.pool.ntp.org > /dev/null 2>&1`
 
这样我们就完成了关于设置修改CentOS系统时区的问题了。

### 四、加入定时计划任务

`crontab -e`

比如说，每隔10分钟同步一下时钟(太频繁了!)

`0-59/10 * * * * /usr/sbin/ntpdate us.pool.ntp.org | logger -t NTP`

指定每星期日的6:30执行ls命令[注：0表示星期天]

`30 6 * * 0 /usr/sbin/ntpdate us.pool.ntp.org | logger -t NTP`

使用ntpdate的遇到这样的错误提示:

`no server suitable for synchronization found`

很可能是防火墙封锁了udp的123端口, 如果关闭的防火墙问题依旧, 很可能是上层路由的设置有问题, 如果这种情况, 我们就只能通过tcp来更新时间啦, 那肯定是rdate
 
#### 查看时间服务器的时间:

`# rdate time-b.nist.gov`
 
#### 设置时间和时间服务器同步:

`# rdate -s time-b.nist.gov`

下面附送系列时间服务器的列表：

    time.nist.gov
    time-b.nist.gov
    216.118.116.105
    rdate.darkorb.net
    202.106.196.19
    time-b.timefreq.bldrdoc.gov
