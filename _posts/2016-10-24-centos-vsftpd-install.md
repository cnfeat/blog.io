---
layout: post
title: CentOS 5 系统下 vsftpd 配置，并建立虚拟用户教程
date: 2016-10-24
categories: blog
tags: [centos,ftp]
description: CentOS 5 系统下 vsftpd 配置，并建立虚拟用户教程

---

环境：CentOS 5.0 操作系统（CentOS 6 同理）

## 一.安装：

### 1.安装Vsftpd服务相关部件：

`[root@KcentOS5 ~]# yum install vsftpd*`
 
### 2.确认安装PAM服务相关部件：

`[root@KcentOS5 ~]# yum install pam*`

开发包，其实不装也没有关系，主要的目的是确认PAM。
 
### 3.安装DB4部件包：

这里要特别安装一个db4的包，用来支持文件数据库。

`[root@KcentOS5 ~]# yum install db4*`
 
## 二.系统帐户

### 1.建立Vsftpd服务的宿主用户：

`[root@KcentOS5 ~]# useradd vsftpd -s /sbin/nologin`

默认的Vsftpd的服务宿主用户是root，但是这不符合安全性的需要。

这里建立名字为vsftpd的用户，用他来作为支持Vsftpd的服务宿主用户。

由于该用户仅用来支持Vsftpd服务用，因此没有许可他登陆系统的必要，并设定他为不能登陆系统的用户。
 
### 2.建立Vsftpd虚拟宿主用户：

(如有nginx或apache等网络服务，直接用nginx等网站用户也可以，看具体需求。如已经存在，不需要重复添加)

`[root@KcentOS5 nowhere]# useradd overlord -s /sbin/nologin`

Vsftp的虚拟用户并不是系统用户，也就是说这些FTP的用户在系统中是不存在的。

他们的总体权限其实是集中寄托在一个在系统中的某一个用户身上的，所谓Vsftpd的虚拟宿主用户，就是这样一个支持着所有虚拟用户的宿主用户。

由于他支撑了FTP的所有虚拟的用户，那么他本身的权限将会影响着这些虚拟的用户，因此，处于安全性的考虑，也要注意对该用户的权限的控制，该用户也绝对没有登陆系统的必要，这里也设定他为不能登陆系统的用户。
 
## 三.调整Vsftpd的配置文件：

### 1.编辑配置文件前先备份

`[root@KcentOS5 ~]# cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak`

### 2.编辑主配置文件Vsftpd.conf

`[root@KcentOS5 ~]# vi /etc/vsftpd/vsftpd.conf`


    anonymous_enable=NO
    # 设定不允许匿名访问

    local_enable=YES
    # 设定本地用户可以访问。注意：主要是为虚拟宿主用户，如果该项目设定为NO那么所有虚拟用户将无法访问。

    write_enable=YES
    # 设定可以进行写操作。

    local_umask=022
    # 设定上传后文件的权限掩码。

    anon_upload_enable=NO
    # 禁止匿名用户上传。

    anon_mkdir_write_enable=NO
    # 禁止匿名用户建立目录。

    dirmessage_enable=YES
    # 设定开启目录标语功能。

    xferlog_enable=YES
    # 设定开启日志记录功能。

    connect_from_port_20=YES
    # 设定端口20进行数据连接。

    chown_uploads=NO
    # 设定禁止上传文件更改宿主。

    xferlog_file=/var/log/vsftpd.log
    # 设定Vsftpd的服务日志保存路径。注意，该文件默认不存在。必须要手动touch出来，并且由于这里更改了Vsftpd的服务宿主用户为手动建立的Vsftpd。必须注意给与该用户对日志的写入权限，否则服务将启动失败。

    xferlog_std_format=YES
    # 设定日志使用标准的记录格式。

    nopriv_user=vsftpd
    # 设定支撑Vsftpd服务的宿主用户为手动建立的Vsftpd用户。注意，一旦做出更改宿主用户后，必须注意一起与该服务相关的读写文件的读写赋权问题。比如日志文件就必须给与该用户写入权限等。

    async_abor_enable=YES
    # 设定支持异步传输功能。

    ascii_upload_enable=YES
    ascii_download_enable=YES
    # 设定支持ASCII模式的上传和下载功能。

    chroot_list_enable=YES
    # 禁止用户登出自己的FTP主目录。

    chroot_list_file=/etc/vsftpd/chroot_list
    # 先新建此路径的文件，再添加虚拟用户名字进去，一行一个！

    ls_recurse_enable=NO
    # 禁止用户登陆FTP后使用"ls -R"的命令。该命令会对服务器性能造成巨大开销。如果该项被允许，那么挡多用户同时使用该命令时将会对该服务器造成威胁。

    listen=YES
    # 设定该Vsftpd服务工作在StandAlone模式下。顺便展开说明一下，所谓StandAlone模式就是该服务拥有自己的守护进程支持，在ps -A命令下我们将可用看到vsftpd的守护进程名。如果不想工作在StandAlone模式下，则可以选择SuperDaemon模式，在该模式下 vsftpd将没有自己的守护进程，而是由超级守护进程Xinetd全权代理，与此同时，Vsftp服务的许多功能将得不到实现。

    pam_service_name=vsftpd
    # 设定PAM服务下Vsftpd的验证配置文件名。因此，PAM验证将参考/etc/pam.d/下的vsftpd文件配置。

    userlist_enable=YES
    # 设定userlist_file中的用户将不得使用FTP。

    tcp_wrappers=YES
    # 设定支持TCP Wrappers
     
    # 以下这些是关于Vsftpd虚拟用户支持的重要配置项目。默认Vsftpd.conf中不包含这些设定项目，需要自己手动添加配置。

    guest_enable=YES
    # 设定启用虚拟用户功能。

    guest_username=overlord
    # 指定虚拟用户的宿主用户(也可设为nginx, apache等，看具体需求)

    virtual_use_local_privs=YES
    # 设定虚拟用户的权限符合他们的宿主用户。

    user_config_dir=/etc/vsftpd/vconf
    # 设定虚拟用户个人Vsftp的配置文件存放路径。也就是说，这个被指定的目录里，将存放每个Vsftp虚拟用户个性的配置文件，一个需要注意的地方就是这些配置文件名必须和虚拟用户名相同。

保存退出。
 
### 3.建立Vsftpd的日志文件，并更该属主为Vsftpd的服务宿主用户：

    [root@KcentOS5 ~]# touch /var/log/vsftpd.log
    [root@KcentOS5 ~]# chown vsftpd.vsftpd /var/log/vsftpd.log
 
### 4.建立虚拟用户配置文件存放路径：

`[root@KcentOS5 ~]# mkdir /etc/vsftpd/vconf/`
 
## 四.制作虚拟用户数据库文件

### 1.先建立虚拟用户名单文件：

`[root@KcentOS5 ~]# touch /etc/vsftpd/virtusers`

建立了一个虚拟用户名单文件，这个文件就是来记录vsftpd虚拟用户的用户名和口令的数据文件，我这里给它命名为virtusers。为了避免文件的混乱，我把这个名单文件就放置在 **/etc/vsftpd/** 下。
 
### 2.编辑虚拟用户名单文件：

`[root@KcentOS5 ~]# vi /etc/vsftpd/virtusers`

    user1
    password1
    user2
    password2

编辑这个虚拟用户名单文件，在其中加入用户的用户名和口令信息。格式很简单：“一行用户名，一行口令”。
 
### 3.生成虚拟用户数据文件：

[root@KcentOS5 ~]

`# db_load -T -t hash -f /etc/vsftpd/virtusers  /etc/vsftpd/virtusers.db`

#### 察看 db4 的 db_load 命令使用方法：

    [root@KSRV2 vsftpd]# db_load
    usage: db_load [-nTV] [-c name=value] [-f file]
    [-h home] [-P password] [-t btree | hash | recno | queue] db_file
    usage: db_load -r lsn | fileid [-h home] [-P password] db_file

选项-T，允许应用程序能够将文本文件转译载入进数据库。

由于我们之后是将虚拟用户的信息以文件方式存储在文件里的，为了让Vsftpd这个应用程序能够通过文本来载入用户数据，必须要使用这个选项。如果指定了选项-T，那么一定要追跟子选项-t。

子选项-t，追加在在-T选项后，用来指定转译载入的数据库类型。-t可以指定的数据类型有Btree、Hash、Queue和Recon数据库。

这里，接下来我们需要指定的是Hash型。
 
### 4.察看生成的虚拟用户数据文件

    [root@KcentOS5 ~]# ll /etc/vsftpd/virtusers.db
    -rw-r--r-- 1 root root 12288 Sep 16 03:51 /etc/vsftpd/virtusers.db

注意：以后再要添加虚拟用户时，只需要按照“一行用户名，一行口令”的格式将新用户名和口令添加进虚拟用户名单文件。还要再执行一遍“ db_load -T -t hash -f 虚拟用户名单文件 虚拟用户数据库文件.db ”的命令使其生效才可以！
 
## 五.设定PAM验证文件，并指定虚拟用户数据库文件进行读取

### 1.察看原来的Vsftp的PAM验证配置文件：

`[root@KcentOS5 ~]# cat /etc/pam.d/vsftpd`
 
### 2.在编辑前做好备份：

`[root@KcentOS5 ~]# cp /etc/pam.d/vsftpd /etc/pam.d/vsftpd.bak`
 
### 3.编辑Vsftpd的PAM验证配置文件

`[root@KcentOS5 ~]# vi /etc/pam.d/vsftpd`

    #%PAM-1.0
    #32位系统内容：
    auth  sufficient  /lib/security/pam_userdb.so  db=/etc/vsftpd/virtusers
    account  sufficient  /lib/security/pam_userdb.so  db=/etc/vsftpd/virtusers
    
    #64位系统内容：
    auth  sufficient  /lib64/security/pam_userdb.so  db=/etc/vsftpd/virtusers
    account  sufficient  /lib64/security/pam_userdb.so  db=/etc/vsftpd/virtusers

PS：放在文件内容最上面

以上两条是手动添加的，内容是对虚拟用户的安全和帐户权限进行验证。

这里的auth是指对用户的用户名口令进行验证。

这里的accout是指对用户的帐户有哪些权限哪些限制进行验证。

其后的sufficient表示充分条件，也就是说，一旦在这里通过了验证，那么也就不用经过下面剩下的验证步骤了。

相反，如果没有通过的话，也不会被系统立即挡之门外，因为sufficient的失败不决定整个验证的失败，意味着用户还必须将经历剩下来的验证审核。

再后面的 /lib/security/pam_userdb.so 表示该条审核将调用 pam_userdb.so 这个库函数进行。
最后的 db=/etc/vsftpd/virtusers 则指定了验证库函数将到这个指定的数据库中调用数据进行验证。
 
## 六.虚拟用户的配置

### 1.规划好虚拟用户的主路径：

`[root@KcentOS5 ~]# mkdir /home/ftp/`
 
### 2.建立测试用户的FTP用户目录：

`[root@KcentOS5 ~]# mkdir /home/ftp/mmd`
 
### 3.建立虚拟用户配置文件模版：

`[root@KcentOS5 ~]# cp /etc/vsftpd/vsftpd.conf.bak /etc/vsftpd/vconf/vconf.tmp`
 
### 4.定制虚拟用户模版配置文件：

`[root@KcentOS5 ~]# vi /etc/vsftpd/vconf/vconf.tmp`

    local_root=/home/ftp/virtuser
    # 指定虚拟用户的具体主路径。

    anonymous_enable=NO
    # 设定不允许匿名用户访问。

    write_enable=YES
    # 设定允许写操作。

    local_umask=022
    # 设定上传文件权限掩码。

    anon_upload_enable=NO
    # 设定不允许匿名用户上传。

    anon_mkdir_write_enable=NO
    # 设定不允许匿名用户建立目录。
    
    idle_session_timeout=600
    # 设定空闲连接超时时间。

    data_connection_timeout=120
    # 设定单次连续传输最大时间。

    max_clients=10
    # 设定并发客户端访问个数。

    max_per_ip=5
    # 设定单个客户端的最大线程数，这个配置主要来照顾Flashget、迅雷等多线程下载软件。

    local_max_rate=50000
    # 设定该用户的最大传输速率，单位b/s。

这里将原vsftpd.conf配置文件经过简化后保存作为虚拟用户配置文件的模版。

这里将并不需要指定太多的配置内容，主要的框架和限制交由 Vsftpd 的主配置文件 vsftpd.conf 来定义。

即虚拟用户配置文件当中没有提到的配置项目将参考主配置文件中的设定。

而在这里作为虚拟用户的配置文件模版只需要留一些和用户流量控制，访问方式控制的配置项目就可以了。

这里的 **关键项是 local_root** 这个配置，用来指定这个虚拟用户的FTP主路径。
 
### 5.更改虚拟用户的主目录的属主为虚拟宿主用户：

`[root@KcentOS5 ~]# chown -R overlord.overlord /home/ftp/`
 
### 6.检查权限：

`[root@KcentOS5 ~]# ll /home/ftp/`
 
## 七.给测试用户定制：

### 1.从虚拟用户模版配置文件复制：

`[root@KcentOS5 ~]# cp /etc/vsftpd/vconf/vconf.tmp /etc/vsftpd/vconf/xxx`
 
### 2.针对具体用户进行定制：

`[root@KcentOS5 ~]# vi /etc/vsftpd/vconf/xxx`


    local_root=/home/ftp/xxx
    anonymous_enable=NO
    write_enable=YES
    local_umask=022
    anon_upload_enable=NO
    anon_mkdir_write_enable=NO
    idle_session_timeout=300
    data_connection_timeout=90
    max_clients=1
    max_per_ip=1
    local_max_rate=25000

## 八. 新建 目录访问限制 文件，把需要限制目录的虚拟用户名添加进去：

`[root@KcentOS5 ~]# vi /etc/vsftpd/chroot_list`
 
## 九.启动服务：

    [root@KcentOS5 ~]# service vsftpd start
    Starting vsftpd for vsftpd:[ OK ]