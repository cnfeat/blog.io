---
layout: post
title: linux 定时任务的设置
date: 2016-11-17
categories: blog
tags: [linux]
description: linux 定时任务的设置

---

## 为当前用户创建cron服务

### 1.  键入 `crontab  -e ` 编辑crontab服务文件，例如内容如下：

`*/2 * * * * /bin/bash /home/admin/deleteFile.sh`

保存并退出。
 
### 2. 查看该用户下的crontab服务是否创建成功， 使用命令： 

`crontab  -l`
 
### 3. 启动 crontab 服务，不同版本linux系统启动服务的命令可能不同

    sudo service crond start （或）
    service cron start
 
### 4. 查看服务是否已经运行用

`ps -ax | grep cron`

### 5. crontab命令参数与说明:

    crontab -u //设定某个用户的cron服务，一般root用户在执行这个命令的时候需要此参数 
    crontab -l //列出某个用户cron服务的详细内容
    crontab -r //删除某个用户的cron服务
    crontab -e //编辑某个用户的cron服务

比如说 root 查看自己的 cron 设置: `crontab -u root -l`

再例如 root 想删除 fred 的 cron 设置:`crontab -u fred -r`

### 6. cron文件语法:

    分        小时           日       月      星期     命令
    0-59      0-23          1-31    1-12     0-6     command

(以上为取值范围,星期栏 0 表示周日，一般一行对应一个任务)

##### 几个特殊符号的含义:

    “ * ” 代表取值范围内的数字,
    “ / ” 代表”每”,
    “ - ” 代表从某个数字到某个数字,
    “ , ” 分开几个离散的数字

##### 使用特殊字符串来节省时间

使用以下 8 个特殊字符串中的其中一个替代头五个字段，这样不但可以节省你的时间，还可以提高可读性。

    特殊字符	       含义

    @reboot	       在每次启动时运行一次
    @yearly	       每年运行一次,等同于 “ 0 0 1 1 * ”
    @annually	   (同 @yearly)
    @monthly	   每月运行一次, 等同于 “ 0 0 1 * * ”
    @weekly	       每周运行一次, 等同于 “ 0 0 * * 0 ”
    @daily	       每天运行一次, 等同于 “ 0 0 * * * ”
    @midnight	   (同 @daily)
    @hourly	       每小时运行一次, 等同于 “ 0 * * * * ”

如：每小时运行一次 ntpdate 命令

`@hourly /path/to/ntpdate`

### 7. 任务调度执行结果的转向

如：每天 5：30 执行 ls 命令，并把结果输出到 /jp/test 文件中

`30 5 * * * ls >/jp/test 2>&1`

2>&1 表示执行结果及错误信息。

### 8. 备份定时任务

    # crontab -l > /path/to/file
    # crontab -u user -l > /path/to/file


### 备注：

1、  在crontab中，#代表了注释

    #每隔10分钟执行一次  
    */10 * * * * tesh.sh  

2、  当程式在你所指定的时间执行后，系统会寄一封信给你，显示该程式执行的内容，若是你不希望收到这样的信，请在每一条命令后，空一格，再加上 **> /dev/null 2>&1** 即可。如：

`30 5 * * * ls > /dev/null 2>&1`

3、  用户的定时任务文件为 **/var/spool/cron/用户名**

`crontab -e` 命令将相当于 `vim /var/spool/cron/用户名`

4、  cron服务每分钟不仅要读一次/var/spool/cron内的所有文件，还需要读一次 /etc/crontab,因此我们配置这个文件也能运用cron服务做一些事情。用crontab配置是针对某个用户的，而编辑/etc/crontab是针对系统的任务。