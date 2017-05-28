---
layout: post
title: Linux命令行访问网页，curl浏览器详细用法
date: 2017-05-28
categories: blog
tags: [linux]
description: Linux命令行访问网页，curl浏览器详细用法
---

### 在liux模式下用linux命令浏览网页的方法：

用linux命令：lynx，links 浏览 web，

用linux命令：lftp，wget，curl 等上传/下载软件。
 
## CURL --- 命令行浏览器

### 1) 一般使用方法

二话不说，先从这里开始吧！

	curl http://www.yahoo.com

回车之后，www.yahoo.com 的html就显示出来了。

### 2) 下载页面

**要想把读过来页面存下来**：

方法一：

	curl http://www.yahoo.com > page.html

方法二：

用curl的内置option就好，存下http的结果，用这个 `option: -o`

	curl -o page.html http://www.yahoo.com

这样，你就可以看到屏幕上出现一个下载页面进度指示。等进展到100%，自然就OK咯

### 3) proxy设定

使用curl的时候，用这个option可以指定http访问所使用的proxy服务器及其端口： `-x`

	curl -x 123.45.67.89:1080 -o page.html http://www.yahoo.com

### 4) 使用cookie来记录session信息

我们来学习这个`option: -D`，这个是把http的response里面的cookie信息存到一个特别的文件中去

	curl -x 123.45.67.89:1080 -o page.html -D cookie0001.txt http://www.yahoo.com

这样，当页面被存到page.html的同时，cookie信息也被存到了cookie0001.txt里面了

### 5） 使用上次留下的cookie信息

那么，下一次访问的时候，如何继续使用上次留下的cookie信息呢？

这次我们使用这个option来把上次的cookie信息追加到http request里面去： `-b`

	curl -x 123.45.67.89:1080 -o page1.html -D cookie0002.txt -b cookie0001.txt http://www.yahoo.com

这样，我们就可以几乎模拟所有的IE操作，去访问网页了！

### 6） 设定浏览器信息

在curl给我们提供了一个有用的option，可以让我们随意指定自己这次访问所**宣称的自己的浏览器信息**： `-A`

	curl -A "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0)" -x 123.45.67.89:1080 -o page.html -D cookie0001.txt http://www.yahoo.com

这样，服务器端接到访问的要求，会认为你是一个运行在Windows 2000上的IE6.0，嘿嘿嘿，其实也许你用的是苹果机呢！

而 `"Mozilla/4.73 [en] (X11; U; Linux 2.2; 15 i686"` 则可以告诉对方你是一台PC上跑着的Linux，用的是Netscape 4.73。

### 7） http访问的referer

另外一个服务器端常用的限制方法，就是检查http访问的referer。比如你先访问首页，再访问里面所指定的下载页，这第二次访问的referer地址就是第一次访问成功后的页面地址。这样，服务器端只要发现对下载页面某次访问的referer地址不是首页的地址，就可以断定那是个盗连了。

幸好curl给我们提供了**设定referer**的option： `-e`

	curl -A "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0)" -x 123.45.67.89:1080 -e "mail.yahoo.com" -o page.html -D cookie0001.txt http://www.yahoo.com

这样，就可以骗对方的服务器，你是从mail.yahoo.com点击某个链接过来的了

### 8） 利用curl 下载文件

刚才讲过了，下载页面到一个文件里，可以使用 -o ，下载文件也是一样。

比如， 

	curl -o 1.jpg http://cgi2.tky.3web.ne.jp/~zzh/screen1.JPG

这里教大家一个新的 `option： -O`

大写的O，这么用： 

	curl -O http://cgi2.tky.3web.ne.jp/~zzh/screen1.JPG

这样，就可以按照服务器上的文件名，自动存在本地了！

再来一个更好用的。

如果screen1.JPG以外还有screen2.JPG、screen3.JPG、....、screen10.JPG需要下载

在curl里面，这么写就可以了：

	curl -O http://cgi2.tky.3web.ne.jp/~zzh/screen[1-10].JPG

### 9） 再来，我们继续讲解下载！

	curl -O http://cgi2.tky.3web.ne.jp/~{zzh,nick}/[001-201].JPG

这样产生的下载，就是

	~zzh/001.JPG
	~zzh/002.JPG
	...
	~zzh/201.JPG
	~nick/001.JPG
	~nick/002.JPG
	...
	~nick/201.JPG

够方便的了吧？

咦？高兴得太早了。

由于zzh/nick下的文件名都是001，002...，201，下载下来的文件重名，后面的把前面的文件都给覆盖掉了~~~

没关系，我们还有更狠的！

	curl -o #2_#1.jpg http://cgi2.tky.3web.ne.jp/~{zzh,nick}/[001-201].JPG

以上是自定义文件名的下载。

	#1 是变量，指的是{zzh,nick}这部分，第一次取值zzh，第二次取值nick
	#2 代表的变量，则是第二段可变部分---[001-201]，取值从001逐一加到201

这样，自定义出来下载下来的文件名，就变成了这样：

原来： ~zzh/001.JPG ---> 下载后： 001-zzh.JPG

原来： ~nick/001.JPG ---> 下载后： 001-nick.JPG

这样一来，就不怕文件重名了。

### 9）继续讲下载

我们平时在windows平台上，flashget这样的工具可以帮我们分块并行下载，还可以断线续传。

curl在这些方面也不输给谁，

比如我们下载screen1.JPG中，突然掉线了，我们就可以这样开始续传

	curl -c -O http://cgi2.tky.3wb.ne.jp/~zzh/screen1.JPG

**分块下载**，我们使用这个option就可以了： `-r`

举例说明

比如我们有一个http://cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3 要下载

我们就可以用这样的命令：

	curl -r 0-10240 -o "zhao.part1" http:/cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3 &\
	curl -r 10241-20480 -o "zhao.part1" http:/cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3 &\
	curl -r 20481-40960 -o "zhao.part1" http:/cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3 &\
	curl -r 40961- -o "zhao.part1" http:/cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3

这样就可以分块下载啦。

不过你需要自己**把这些破碎的文件合并起来**

如果你用UNIX或苹果，用 

	cat zhao.part* > zhao.mp3

就可以。

如果用的是Windows，用 `copy /b` 来解决吧

上面讲的都是http协议的下载，其实ftp也一样可以用。

用法：

	curl -u name:passwd ftp://ip:port/path/file

或者大家熟悉的

	curl ftp://name:passwd@ip:port/path/file

### 10) 上传

说完了下载，接下来讲上传

上传的option是 `-T`

比如我们向ftp传一个文件： 

	curl -T localfile -u name:passwd ftp://upload_site:port/path/

当然，向http服务器上传文件也可以

比如 

	curl -T localfile http://cgi2.tky.3web.ne.jp/~zzh/abc.cgi

注意，这时候，使用的协议是HTTP的 PUT method

http提交一个表单，比较常用的是`POST模式`和`GET模式`

`GET模式`什么option都不用，只需要把变量写在url里面就可以了

比如：

	curl http://www.yahoo.com/login.cgi?user=nickwolfe&password=12345

而`POST模式`的option则是 `-d`

比如，

	curl -d "user=nickwolfe&password=12345" http://www.yahoo.com/login.cgi

就相当于向这个站点发出一次登陆申请~~~~~

到底该用 GET模式 还是 POST模式，要看对面服务器的程序设定。

一点需要注意的是，**POST模式下的文件上的文件上传**，比如

	<form method="POST" enctype="multipar/form-data" action="http://cgi2.tky.3web.ne.jp/~zzh/up_file.cgi">
	<input type=file name=upload>
	<input type=submit name=nick value="go">
	</form>

这样一个HTTP表单，我们要用curl进行模拟，就该是这样的语法：

	curl -F upload=@localfile -F nick=go http://cgi2.tky.3web.ne.jp/~zzh/up_file.cgi

其实curl还有很多很多技巧和用法

比如 **https的时候使用本地证书**，就可以这样

	curl -E localcert.pem https://remote_server

再比如，你还可以**用curl通过dict协议去查字典**~~~~~

	curl dict://dict.org/d:computer

注：内容转载自网络