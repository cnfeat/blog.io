---
layout: post
title: vsftpd 实用配置手册
date: 2016-10-27
categories: blog
tags: [ftp]
description: vsftpd 实用配置手册

---

### vsftpd配置参数详细整理

     #接受匿名用户 
     anonymous_enable=YES 
     #匿名用户login时不询问口令 
     no_anon_password=YES 
     #匿名用户主目录 
     anon_root=(none) 
     #接受本地用户 
     local_enable=YES 
     #本地用户主目录 
     local_root=(none) 
     #如果匿名用户需要密码,那么使用banned_email_file里面的电子邮件地址的用户不能登录 
     deny_email_enable=YES 
     #仅在没有pam验证版本时有用,是否检查用户有一个有效的shell来登录 
     check_shell=YES 
     #若启用此选项,userlist_deny选项才被启动 
     userlist_enable=YES 
     #若为YES,则userlist_file中的用户将不能登录,为NO则只有userlist_file的用户可以登录 
     userlist_deny=NO 
     #如果和chroot_local_user一起开启,那么用户锁定的目录来自/etc/passwd每个用户指定的目录(这个不是很清楚,很哪位熟悉的指点一下) 
     passwd_chroot_enable=NO 
     #定义匿名登入的使用者名称。默认值为ftp。 
     ftp_username=FTP 
 
 #################用户权限控制############### 

     #可以上传(全局控制). 
     write_enable=YES 
     #本地用户上传文件的umask 
     local_umask=022 
     #上传文件的权限配合umask使用 
     #file_open_mode=0666 
     #匿名用户可以上传 
     anon_upload_enable=NO 
     #匿名用户可以建目录 
     anon_mkdir_write_enable=NO 
     匿名用户其它的写权利(更改权限?) 
     anon_other_write_enable=NO 
     如果设为YES，匿名登入者会被允许下载可阅读的档案。默认值为YES。 
     anon_world_readable_only=YES 
     #如果开启,那么所有非匿名登陆的用户名都会被切换成guest_username指定的用户名 
     #guest_enable=NO 
     所有匿名上传的文件的所属用户将会被更改成chown_username 
     chown_uploads=YES 
     匿名上传文件所属用户名 
     chown_username=lightwiter 
     #如果启动这项功能，则所有列在chroot_list_file之中的使用者不能更改根目录 
     chroot_list_enable=YES 
     #允许使用"async ABOR"命令,一般不用,容易出问题 
     async_abor_enable=YES 
     管控是否可用ASCII 模式上传。默认值为NO。 
     ascii_upload_enable=YES 
     #管控是否可用ASCII 模式下载。默认值为NO。 
     ascii_download_enable=YES 
     #这个选项必须指定一个空的数据夹且任何登入者都不能有写入的权限，当vsftpd 不需要file system 的权限时，就会将使用者限制在此数据夹中。默认值为/usr/share/empty 
     secure_chroot_dir=/usr/share/empty 
 
 ###################超时设置################## 

     #空闲连接超时 
     idle_session_timeout=600 
     #数据传输超时 
     data_connection_timeout=120 
     #PAVS请求超时 
     ACCEPT_TIMEOUT=60 
     #PROT模式连接超时 
     connect_timeout=60 
 
 ################服务器功能选项############### 

     #开启日记功能 
     xferlog_enable=YES 
     #使用标准格式 
     xferlog_std_format=YES 
     #当xferlog_std_format关闭且本选项开启时,记录所有ftp请求和回复,当调试比较有用. 
     #log_ftp_protocol=NO 
     #允许使用pasv模式 
     pasv_enable=YES 
     #关闭安全检查,小心呀. 
     #pasv_promiscuous+NO 
     #允许使用port模式 
     #port_enable=YES 
     #关闭安全检查 
     #prot_promiscuous 
     #开启tcp_wrappers支持 
     tcp_wrappers=YES 
     #定义PAM 所使用的名称，预设为vsftpd。 
     pam_service_name=vsftpd 
     #当服务器运行于最底层时使用的用户名 
     nopriv_user=nobody 
     #使vsftpd在pasv命令回复时跳转到指定的IP地址.(服务器联接跳转?) 
     pasv_address=(none) 
 
 #################服务器性能选项############## 

     #是否能使用ls -R命令以防止浪费大量的服务器资源 
     #ls_recurse_enable=YES 
     #是否使用单进程模式 
     #one_process_model 
     #绑定到listen_port指定的端口,既然都绑定了也就是每时都开着的,就是那个什么standalone模式 
     listen=YES 
     #当使用者登入后使用ls -al 之类的指令查询该档案的管理权时，预设会出现拥有者的UID，而不是该档案拥有者的名称。若是希望出现拥有者的名称，则将此功能开启。 
     text_userdb_names=NO 
     #显示目录清单时是用本地时间还是GMT时间,可以通过mdtm命令来达到一样的效果 
     use_localtime=NO 
     #测试平台优化 
     #use_sendfile=YES 
 
 ################信息类设置################ 

     #login时显示欢迎信息.如果设置了banner_file则此设置无效 
     ftpd_banner=欢迎来到湖南三辰Fake-Ta FTP 网站. 
     #允许为目录配置显示信息,显示每个目录下面的message_file文件的内容 
     dirmessage_enable=YES 
     #显示会话状态信息,关! 
     #setproctitle_enable=YES 
 
 ############## 文件定义 ################## 

     #定义不能更改用户主目录的文件 
     chroot_list_file=/etc/vsftpd/vsftpd.chroot_list 
     #定义限制/允许用户登录的文件 
     userlist_file=/etc/vsftpd/vsftpd.user_list 
     #定义登录信息文件的位置 
     banner_file=/etc/vsftpd/banner 
     #禁止使用的匿名用户登陆时作为密码的电子邮件地址 
     banned_email_file=/etc/vsftpd.banned_emails 
     #日志文件位置 
     xferlog_file=/var/log/vsftpd.log 
     #目录信息文件 
     message_file=.message 
 
 ##############目录定义 ################# 

     #定义用户配置文件的目录 
     user_config_dir=/etc/vsftpd/userconf 
     #定义本地用户登陆的根目录,注意定义根目录可以是相对路径也可以是绝对路径.相对路径是针对用户家目录来说的. 
     local_root=webdisk #此项设置每个用户登陆后其根目录为/home/username/webdisk 
     #匿名用户登陆后的根目录 
     anon_root=/var/ftp 
 
 #############用户连接选项################# 

     #可接受的最大client数目 
     max_clients=100 
     #每个ip的最大client数目 
     max_per_ip=5 
     #使用标准的20端口来连接ftp 
     connect_from_port_20=YES 
     #绑定到某个IP,其它IP不能访问 
     listen_address=192.168.0.2 
     #绑定到某个端口 
     #listen_port=2121 
     #数据传输端口 
     #ftp_data_port=2020 
     #pasv连接模式时可以使用port 范围的上界，0 表示任意。默认值为0。 
     pasv_max_port=0 
     #pasv连接模式时可以使用port 范围的下界，0 表示任意。默认值为0。 
     pasv_min_port=0 
 
 ##############数据传输选项################# 

     #匿名用户的传输比率(b/s) 
     anon_max_rate=51200 
     #本地用户的传输比率(b/s) 
     local_max_rate=5120000 
 
 ######################################## 

 别外,如果要对每个用户进行单独的控制,只需要在user_config_dir中建立username文件,内容为数据传输和用户权利里面设置个人的合适的选项,用户自定义文件同样适合用pam支持的虚拟用户

### 附: FTP 数字代码的意义 
    
     110 重新启动标记应答。 
     120 服务在多久时间内ready。 
     125 数据链路埠开启，准备传送。 
     150 文件状态正常，开启数据连接端口。 
     200 命令执行成功。 
     202 命令执行失败。 
     211 系统状态或是系统求助响应。 
     212 目录的状态。 
     213 文件的状态。 
     214 求助的讯息。 
     215 名称系统类型。 
     220 新的联机服务ready。 
     221 服务的控制连接埠关闭，可以注销。 
     225 数据连结开启，但无传输动作。 
     226 关闭数据连接端口，请求的文件操作成功。 
     227 进入passive mode。 
     230 使用者登入。 
     250 请求的文件操作完成。 
     257 显示目前的路径名称。 
     331 用户名称正确，需要密码。 
     332 登入时需要账号信息。 
     350 请求的操作需要进一部的命令。 
     421 无法提供服务，关闭控制连结。 
     425 无法开启数据链路。 
     426 关闭联机，终止传输。 
     450 请求的操作未执行。 
     451 命令终止：有本地的错误。 
     452 未执行命令：磁盘空间不足。 
     500 格式错误，无法识别命令。 
     501 参数语法错误。 
     502 命令执行失败。 
     503 命令顺序错误。 
     504 命令所接的参数不正确。 
     530 未登入。 
     532 储存文件需要账户登入。 
     550 未执行请求的操作。 
     551 请求的命令终止，类型未知。 
     552 请求的文件终止，储存位溢出。 
     553 未执行请求的的命令，名称不正确。 
