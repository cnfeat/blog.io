---
layout: post
title: linux下rsync命令详解
date: 2017-03-10
categories: blog
tags: [rsync]
description: linux下rsync命令详解
---

### 一、rsync是什么

rsync（remote synchronize）是Liunx/Unix下的一个远程数据同步工具。它可通过LAN/WAN快速同步多台主机间的文件和目录，并适当利用rsync算法（差分编码）以减少数据的传输。

rsync算法并不是每一次都整份传输，而是只传输两个文件的不同部分，因此其传输速度相当快。

除此之外，rsync可拷贝、显示目录属性，以及拷贝文件，并可选择性的压缩以及递归拷贝。

### 二、rsync的工作原理

1. 客户端构造FileList，FileList包含了需要与服务器同步的所有文件信息对name->id（id用来唯一表示文件例如MD5）。

2. 客户端将FileList发送到服务器。

3. 服务器上rsync处理客户端发过来的FileList，构建新的NewFileList。其中根据MD5值比较，删除服务器上已经存在的文件信息对，只保留服务器上不存在或变化的文件。

4. 客户端得到服务器发送过来的NewFileList，然后把NewFileList中的文件重新传输到服务器。

### 三、rsync优点

rsync有以下几个优点：

1. 可以镜像保存整个目录树和文件系统。

2. 可以很容易做到保持原来文件的权限、时间、软硬连接等。

3. 无需特殊权限即可安装。

4. 拥有优化的流程和比较高的文件传输效率。

5. 可以使用shell（rsh、ssh）方式来传输文件。

6. 支持匿名运行。

7. 与scp相比，rsync传输速度绝对远远超过scp的传输速度。

我们在局域网中经常用rsync和scp传输大量mysql数据库文件，发现rsync传输文件速度至少要比scp快20倍以上。

所以**如果需要在Liunx/Unix服务器之间互传海量数据时，建议选择rsync进行传输。**


### 四、rsync命令格式：

Rsync的命令格式可以为以下六种： 

	rsync [OPTION]... SRC DEST 
	rsync [OPTION]... SRC [USER@]HOST:DEST 
	rsync [OPTION]... [USER@]HOST:SRC DEST 
	rsync [OPTION]... [USER@]HOST::SRC DEST 
	rsync [OPTION]... SRC [USER@]HOST::DEST 
	rsync [OPTION]... rsync://[USER@]HOST[:PORT]/SRC [DEST] 

对应于以上六种命令格式，rsync有六种不同的工作模式： 

1. **拷贝本地文件**。当SRC和DES路径信息都不包含有单个冒号":"分隔符时就启动这种工作模式。如：`rsync -a /data /backup`

2. 使用一个远程shell程序(如rsh、ssh)来实现**将本地机器的内容拷贝到远程机器**。当DST路径地址包含单个冒号":"分隔符时启动该模式。如：`rsync -avz *.c foo:src`

3. 使用一个远程shell程序(如rsh、ssh)来实现**将远程机器的内容拷贝到本地机器**。当SRC地址路径包含单个冒号":"分隔符时启动该模式。如：`rsync -avz foo:src/bar /data`

4. **从远程rsync服务器中拷贝文件到本地机**。当SRC路径信息包含"::"分隔符时启动该模式。如：`rsync -av root@172.16.78.192::www /databack`

5. **从本地机器拷贝文件到远程rsync服务器中**。当DST路径信息包含"::"分隔符时启动该模式。如：`rsync -av /databack root@172.16.78.192::www`

6. **列远程机的文件列表**。这类似于rsync传输，不过只要在命令中省略掉本地机信息即可。如：`rsync -v rsync://172.16.78.192/www`

### 五、rsync参数的具体解释： 

	-v, --verbose 详细模式输出 
	-q, --quiet 精简输出模式 
	-c, --checksum 打开校验开关，强制对文件传输进行校验 
	-a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD 
	-r, --recursive 对子目录以递归模式处理 
	-R, --relative 使用相对路径信息 
	-b, --backup 创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename。可以使用--suffix选项来指定不同的备份文件前缀。 
	--backup-dir 将备份文件(如~filename)存放在在目录下。 
	-suffix=SUFFIX 定义备份文件前缀 
	-u, --update 仅仅进行更新，也就是跳过所有已经存在于DST，并且文件时间晚于要备份的文件。(不覆盖更新的文件) 
	-l, --links 保留软链结 
	-L, --copy-links 想对待常规文件一样处理软链结 
	--copy-unsafe-links 仅仅拷贝指向SRC路径目录树以外的链结 
	--safe-links 忽略指向SRC路径目录树以外的链结 
	-H, --hard-links 保留硬链结 
	-p, --perms 保持文件权限 
	-o, --owner 保持文件属主信息 
	-g, --group 保持文件属组信息 
	-D, --devices 保持设备文件信息 
	-t, --times 保持文件时间信息 
	-S, --sparse 对稀疏文件进行特殊处理以节省DST的空间 
	-n, --dry-run现实哪些文件将被传输 
	-W, --whole-file 拷贝文件，不进行增量检测 
	-x, --one-file-system 不要跨越文件系统边界 
	-B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节 
	-e, --rsh=COMMAND 指定使用rsh、ssh方式进行数据同步 
	--rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息 
	-C, --cvs-exclude 使用和CVS一样的方法自动忽略文件，用来排除那些不希望传输的文件 

	--existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件 
	--delete 删除那些DST中SRC没有的文件 
	--delete-excluded 同样删除接收端那些被该选项指定排除的文件 
	--delete-after 传输结束以后再删除 
	--ignore-errors 即使出现IO错误也进行同步 
	--max-delete=NUM 最多删除NUM个文件 
	--partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输 
	--force 强制删除目录，即使不为空 
	--numeric-ids 不将数字的用户和组ID匹配为用户名和组名 
	--timeout=TIME IP超时时间，单位为秒 
	-I, --ignore-times 不跳过那些有同样的时间和长度的文件 
	--size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间 
	--modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0 
	-T --temp-dir=DIR 在DIR中创建临时文件 
	--compare-dest=DIR 同样比较DIR中的文件来决定是否需要备份 
	-P 等同于 --partial 
	--progress 显示备份过程 
	-z, --compress 对备份的文件在传输时进行压缩处理 

	--exclude=PATTERN 指定排除不需要传输的文件模式  (比如--exclude “./log” --exclude ‘./log/file’)
	--include=PATTERN 指定不排除而需要传输的文件模式 
	--exclude-from=FILE 排除FILE中指定模式的文件
	--include-from=FILE 不排除FILE指定模式匹配的文件
	--files-from=/usr/local/inotify/rsinc.list 只同步指定的文件

	--version 打印版本信息 
	--address 绑定到特定的地址 
	--config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件 
	--port=PORT 指定其他的rsync服务端口 
	--blocking-io 对远程shell使用阻塞IO 笔记本
	-stats 给出某些文件的传输状态 
	--progress 在传输时现实传输过程 
	--log-format=formAT 指定日志文件格式 

	--password-file=FILE 从FILE中得到密码 
	--bwlimit=KBPS 限制I/O带宽，KBytes per second 
	-h, --help 显示帮助信息
