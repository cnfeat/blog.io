---
layout: post
title: Ubuntu 用 vsftpd 配置 FTP 服务器
date: 2016-10-09
categories: blog
tags: [ubuntu]
description: Ubuntu 用 vsftpd 配置 FTP 服务器

---

### 安装ftp

`sudo apt-get install vsftpd`

### 配置vsftpd.conf

`sudo vi /etc/vsftpd.conf`

    #禁止匿名访问
    anonymous_enable=NO
    #接受本地用户
    local_enable=YES
    #允许上传
    write_enable=YES
    #用户只能访问限制的目录
    chroot_local_user=YES
    #设置固定目录，在结尾添加。如果不添加这一行，各用户对应自己的目录，当然这个文件夹自己建
    local_root=/home/ftp

看网上说加一行“pam_service_name=vsftpd”，我看我这个配置文件本来就有，就不管了。

### 添加ftp用户

    sudo useradd -d /home/ftp -M ftpuser
    sudo passwd ftpuser

### 调整文件夹权限

这个是避免“500 OOPS: vsftpd: refusing to run with writable root inside chroot()”

    sudo chmod a-w /home/ftp
    sudo mkdir /home/ftp/data

这样登录之后会看到data文件夹，虽然稍麻烦，原因不表了。。查资料这么辛酸已经不易。。

### 改pam.d/vsftpd

这时候直接用useradd的帐号登录ftp会530 login incorrect

`sudo vi /etc/pam.d/vsftpd`

注释掉 

`#auth    required pam_shells.so`

### 重启vsftpd

`sudo service vsftpd restart`

这时就可以用刚才建的ftpuser这个用户登录ftp了，看到的是local_root设置的/home/ftp，并且限制在该目录。

可以在浏览器用ftp://xxx.xxx.xxx.xxx访问，也可以用ftp软件比如flashFXP，密码就是ftpuser的密码。


----------

#### 如果无法连接ftp服务器，且vsftpd启动不成功，设置/etc/vsftpd.conf：

`listen_ipv6=NO`

#### 有提示：500 OOPS：cannot locate user entry:ftpsecure而不能登录成功

这种原因是ubuntu中没有ftpsecure的用户。

`adduser  ftpsecure`

使用以上命令添加该用户即可。再次登录服务器，登录成功。 

#### 提示：500 OOPS: cannot locate user entry:vsftpd

用命令  **groups vsftpd**  查看发现系统中没有vsftpd组,

于是手动增加vsftpd组和用户:

    # groupadd vsftpd
    # adduser  -g vsftpd -s /sbin/nologin vsftpd

然后重启vsftpd，登陆发现问题解决了。

----------

### 关于用户访问文件夹限制

关于`chroot_local_user、chroot_list_enable、chroot_list_file`这三个文件控制：

首先，`chroot_list_enable`好理解，就是：是否启用`chroot_list_file`配置的文件，如果为YES表示`chroot_list_file`配置的文件生效，否则不生效；

第二，`chroot_list_file`也简单，配置了一个文件路径，默认是`/etc/vsftpd.chroot_list`，该文件中会填入一些账户名称。但是这些账户的意义不是固定的，是跟配置项`chroot_local_user`有关的。后一条中说明；

第三，`chroot_local_user`为 **YES** 表示所有用户都 **不能** 切换到主目录之外其他目录，但是！除了`chroot_list_file`配置的文件列出的用户。`chroot_local_user` 为 **NO** 表示所有用户都 **能** 切换到主目录之外其他目录，但是！除了`chroot_list_file`配置的文件列出的用户。也可以理解为，`chroot_list_file`列出的 **“例外情况”** 的用户。

### 如果客户端登录时候提示“以pasv模式连接失败”

编辑/etc/vsftpd.conf:

`vi /etc/vsftpd.conf`

文件的最后添加

`pasv_promiscuous=YES`

然后再重启vsftpd服务。