---
layout: post
title: Linux下压缩、解压命令大全
date: 2016-11-03
categories: blog
tags: [linux]
description: Linux下压缩、解压命令大全

---

### .tar

    解包：tar xvf FileName.tar
    打包：tar cvf FileName.tar DirName


语法：tar [选项] [档案文件名称] [需要归档的文件]

选项 c，创建档案文件。 

选项 v，verbose模式，即在命令执行过程中显示更多信息。 

选项 f，在命令中指出归档文件名。 

选项 x 可以从档案文件中提取文件

选项t会显示归档文件里面的所有文件

`tar tvf /tmp/my_home_directory.tar` 

**注：tar是打包，不是压缩！**

### .gz

    解压1：gunzip FileName.gz
    解压2：gzip -d FileName.gz
    压缩：gzip FileName

### .tar.gz 和 .tgz

    解压：tar zxvf FileName.tar.gz
    压缩：tar zcvf FileName.tar.gz DirName

处理tar.gz格式的压缩文件时需要添加选项 z
[注：使用gzip要比bzip2快] 
 
将tar.gz文件解压到指定目录

`tar xvfz /tmp/my_home_directory.tar.gz -C /home/ramesh` 

### 压缩时排除目录或文件：

`tar zcvf fd.tar.gz * --exclude=file1 --exclude=dir1`

注意：

1、**--exclude=file1** 而不是 --exclude file1

2、要排除一个目录是 **--exclude=dir1** 而不是--exclude=dir1/

也可以在父目录打包:

`tar zcvf fd.tar.gz pardir --exclude=pardir/file1 --exclude=pardir/dir1`

### .bz2

    解压1：bzip2 -d FileName.bz2
    解压2：bunzip2 FileName.bz2
    压缩： bzip2 -z FileName

### .tar.bz2

    解压：tar jxvf FileName.tar.bz2
    压缩：tar jcvf FileName.tar.bz2 DirName

处理tar.bzip2格式的压缩文件时需要添加 选项 j

[注：使用bzip2会获得比gzip高的压缩率] 

### .bz

    解压1：bzip2 -d FileName.bz
    解压2：bunzip2 FileName.bz
    压缩：未知

### .tar.bz

    解压：tar jxvf FileName.tar.bz
    压缩：未知

### .Z

    解压：uncompress FileName.Z
    压缩：compress FileName

### .tar.Z

    解压：tar Zxvf FileName.tar.Z
    压缩：tar Zcvf FileName.tar.Z DirName


### .zip

    解压：unzip FileName.zip
    压缩：zip FileName.zip DirName

语法: **zip {.zip file-name} {file-names}**

用zip压缩多个文件

`zip var-log-files.zip /var/log/*`

递归地压缩一个目录及目录下的文件

`zip -r var-log-dir.zip /var/log/` 

不解压一个压缩包的情况下看里面的文件

`unzip -l var-log.zip` 

#### zip命令提供了十个压缩等级： 
- 等级0是最低等级，只做归档，不压缩
- 等级1压缩率低，但速度很快 
- 等级6是默认的压缩等级 
- 等级9的压缩率最高，但它耗时也多，除了大文件，我们一般推荐于用等级9 

`zip -9 var-log-files-9.zip /var/log/*` 

使用zip命令的P选项来加密zip文件 

`zip -P mysecurepwd var-log-protected.zip /var/log/*` 

使用e选项来设定密码

`zip -e var-log-protected.zip /var/log/*` 
Enter password: 
Verify password: 

检查zip文件的完整性又不想解压它，使用t选项

`unzip -t var-log.zip` 

### .rar
    
    解压：rar x FileName.rar
    压缩：rar a FileName.rar DirName
 
直接通过yum安装方法：[看这里](wiz://open_document?guid=a27479d9-92b5-4e06-a7a7-38a68ce4117e&kbguid=&private_kbguid=6998e73d-9200-4b1c-8b77-bfcb90a9a248)

rar下载：[http://www.rarsoft.com/download.htm](http://www.rarsoft.com/download.htm)

解压后，请将 rar_static 拷贝到 /usr/bin 目录（其他由$PATH环境变量指定的目录也可以）：

`[root@www2 tmp]# cp rar_static /usr/bin/rar`

### .lha

    解压：lha -e FileName.lha
    压缩：lha -a FileName.lha FileName
 
lha下载：[http://www.infor.kanazawa-it.ac.jp/~ishii/lhaunix/](http://www.infor.kanazawa-it.ac.jp/~ishii/lhaunix/)

解压后，请将 lha 拷贝到 /usr/bin 目录（其他由$PATH环境变量指定的目录也可以）：

`[root@www2 tmp]# cp lha /usr/bin/`


### .rpm

`解包：rpm2cpio FileName.rpm | cpio -div`

### .deb
`解包：ar p FileName.deb data.tar.gz | tar zxf -`

### .tar .tgz .tar.gz .tar.Z .tar.bz .tar.bz2 .zip .cpio .rpm .deb .slp .arj .rar .ace .lha .lzh .lzx .lzs .arc .sda .sfx .lnx .zoo .cab .kar .cpt .pit .sit .sea

    解压：sEx x FileName.*
    压缩：sEx a FileName.* FileName
 
**注意：sEx只是调用相关程序，本身并无压缩、解压功能**

sEx下载： [http://sourceforge.net/projects/sex](http://sourceforge.net/projects/sex)
解压后，请将 sEx 拷贝到 /usr/bin 目录（其他由$PATH环境变量指定的目录也可以）：

`[root@www2 tmp]# cp sEx /usr/bin/`

### gzip 命令

减少文件大小有两个明显的好处，一是可以减少存储空间，二是通过网络传输文件时，可以减少传输的时间。

gzip 是在 Linux 系统中经常使用的一个对文件进行压缩和解压缩的命令，既方便又好用。

语法：**gzip [选项] 压缩（解压缩）的文件名**

该命令的各选项含义如下：

-c 将输出写到标准输出上，并保留原有文件。

-d 将压缩文件解压。

-l 对每个压缩文件，显示下列字段：压缩文件的大小；未压缩文件的大小；压缩比；未压缩文件的名字

-r 递归式地查找指定目录并压缩其中的所有文件或者是解压缩。

-t 测试，检查压缩文件是否完整。

-v 对每一个压缩和解压的文件，显示文件名和压缩比。

-num 用指定的数字 num 调整压缩的速度，-1 或 --fast 表示最快压缩方法（低压缩比），

-9 或--best表示最慢压缩方法（高压缩比）。系统缺省值为 6。

#### 指令实例：

`gzip *`

% 把当前目录下的每个文件压缩成 .gz 文件。
 
`gzip -dv *`

% 把当前目录下每个压缩的文件解压，并列出详细的信息。
 
`gzip -l *`

% 详细显示例1中每个压缩的文件的信息，并不解压。
 
`gzip usr.tar`

% 压缩 tar 备份文件 usr.tar，此时压缩文件的扩展名为.tar.gz。