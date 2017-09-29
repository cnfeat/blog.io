---
layout: post
title: PHP+Apache上传文件大小限制的问题
date: 2017-02-20
categories: blog
tags: [php]
description: PHP+Apache上传文件大小限制的问题
---

### 在用PHP进行文件上传的操作中，需要知道怎么控制上传文件大小的设置，而文件可传大小是受到多种因素制约的，现总结如下：

1、`php.ini: upload_max_filesize` 所上传的文件的最大大小。默认值2M。

2、`php.ini: memory_limit` 本指令设定 了一个脚本所能够申请到的最大内存字节数，默认值8M。如果不需要任何内存上的限制，必须将其设为 -1。如果内存不够，则可能出现错误：Fatal error: Allowed memory size of X bytes exhausted (tried to allocate Y bytes)(一般导入数据库时，如果数据库太大，就会报错，改这个就可以)

3、`php.ini: post_max_size` 设定POST数据所允许的最大大小。此设定也影响到文件上传。要上传大文件，该值必须大于 `upload_max_filesize`。

4、`php.ini: max_execution_time = 30 ;` Maximum execution time of each script, in seconds

5、`php.ini: max_input_time = 60 ;` Maximum amount of time each script may spend parsing request data

6、如果用到mysql的BLOB进行二进制文件存储，则需要设置`my.ini:max_allowed_packet=xxM`

7、`httpd.conf`
在 Apache 里面有一个选项是 `LimitRequestBody`， 这个选项可以限制用户送出的 HTTP 请求内容。这个选项可以在 .htaccess 或 httpd.conf 里使用，而如果在 httpd.conf 内使用，分别可以用在 virtualhost 或目录属性设定。而 `LimitRequestBody` 的设定值是介乎 0 (无限制) 至 2147483647 (2GB)。

**例如：**要在目录 D:/AppServ/www 设定 上传 限制为 100K，可以在 .htaccess 或 httpd.conf 加入以下语句：

	LimitRequestBody 1024000000
	Options Indexes FollowSymLinks MultiViews ExecCGI
	AllowOverride All
	Order allow,deny
	Allow from all

如果透过 .htaccess 设定，储存档案后会立即生效；如透过 httpd.conf 设定，须要重新启动 Apache。

**PHP关于文件上传部分**，特别提到表单隐藏域：`MAX_FILE_SIZE`，意思是接收文件的最大尺寸。文档中给出的例子如下：

	<form enctype="multipart/form-data" action="_URL_" method="POST">
    	<input type="hidden" name="MAX_FILE_SIZE" value="30000">
    	Send this file: <input name="userfile" type="file">
    	<input type="submit" value="Send File">
	</form>

这里设置 `MAX_FILE_SIZE ＝ 30000`，期待一种可能，使得浏览器在传送文件之前能够依此作出预先判断，如果文件尺寸大于30000字节，则不执行实际的POST动作。也就是不往服务器发送文件内容，而是直接在客户端提醒用户“你试图上传的文件超过30000字节”。 
这的确是一个非常棒的主张，但在现实中却暂时无法实现。不是因为这个限制可以“被简单地绕过”，而是**IE和FireFox这两个主流浏览器都不支持这个特性**。PHP的这个建议尚未被采纳。

`MAX_FILE_SIZE` 还有一个用场：

后台PHP会判断接收到的文件大小是否大于这个值，如果超出，`$_FILES['thisfile'] ['error']` 会被设置为 `UPLOAD_ERR_FORM_SIZE(2)`，同时放弃保存临时文件，将`$_FILES['thisfile'] ['size']` 置0。

这个例子，没问题，表现正常，当我试图上传一个40多K的文件时，PHP程序报告“文件超过MAX_FILE_SIZE”。

但是，如果我们将表单中的 `MAX_FILE_SIZE` 从 30000 减少到 1000，情形又如何呢？

上传800字节的文件，正常；
上传40K的文件，PHP报告文件过大，也正常；
上传3000个字节的文件，PHP未报告错误，它成功保存了文件！出乎意料！

问题就出在 `main/rfc1867.c` 中 判断文件是否超长的这部分代码上。php每次从buffer中读取FILLUNIT字节长度的内容后，首先判断“已经读到的内容长度(total_bytes)”是 否大于`MAX_FILE_SIZE`，然后再增加“已经读到的内容长度(total_bytes)”。这样一来，和预计的结果之间至多会有FILLUNIT 字节的误差，而FILLUNIT=1024*5＝5K。（点击bug了解详细内容）

这就是说，当`MAX_FILE_SIZE`<5K时，上传一个大于`MAX_FILE_SIZE`，但是小于5K的文件是没有问题的。
当然，因为这个设置很容易被绕过，所以服务器端编程不应当依赖于`MAX_FILE_SIZE`。而且，5K到底是个很小的数值，对大多数上传文件的表单来说没有影响。

PHP中`post_max_size`，`upload_max_filesize`, `MAX_FILE_SIZE`的设置，和客户端上传给服务器端的流量大小无关。

Apache服务器从客户端接收长度不超过 `LimitRequestBody` 字节数的请求，然后传送给php模块，php模块再决定是否保存成临时文件，设置`$_FILES`全局变量，移交给script进一步处理。

这个Apache的 `LimitRequestBody` 选项缺省值=0，允许Request body的最大字节数是2G（Linux + Apache）

#### 最后还要注意的是：

html本身能够post数据也是有限制的，不能超过2G。

FTP客户端有文件偏移指针的2GB边界限制，未使用特殊编译flag编译的ftp服务器端或者客户端，无论在什么FS中都不支持大于2GB的文件。不知道PHP会不会也有这种情况。