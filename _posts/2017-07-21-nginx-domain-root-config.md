---
layout: post
title: nginx服务器绑定域名和设置根目录的方法
date: 2017-07-21
categories: blog
tags: [nginx]
description: nginx服务器绑定域名和设置根目录的方法
---

### nginx服务器绑定域名以及设置根目录方法：

#### 一、进入nginx安装目录

默认目录一般是 `/etc/nginx/`

或者 `/usr/local/nginx/`

#### 二、找到并打开 nginx 的配置文件

	vi conf/nginx.conf 

找到

	server {
		.....
		.....
	}

这个代码段，这段代码就是用来配置对应站点的。

首先要将域名解析到服务器的IP地址。

### 三、在代码段中找到 `server_name` 这一项

把后面的域名改成我们要绑定的域名。

`root` 这一项是指定网站的根目录，设置成我们需要的目录即可。

![](https://azraelgreen.github.io/img/201707211.png)

#### 四、如果想绑定多个域名怎么办？或是各种二级域名，比如hao.、tools.这样的二级域名？

1、还是将域名解析到服务器

2、整体复制上面`server{}`代码段，重复粘贴到下面即可。这样构造出多个server就是多站点配置了。

注意要复制全，大括号要对称，并且 shell 脚本中大括号和前面的语句之间必须有空格或者换行，这个很重要。

3、很多集成包中会在和配置文件nginx.conf同目录下设置一个 `vhost` 这样的代码虚拟主机的目录，对于绑定多个域名设置多个配置文件，比如aa.conf、bb.conf这些文件。

4、然后在 `nginx.conf` 使用 `include vhost/*.conf` 全部引入。引入相当于所有代码写在nginx.conf中一样，并且不用考虑其他目录的关系，都以nginx.conf为准，这样方便管理。

![](https://azraelgreen.github.io/img/201707212.png)

#### 五、其他规则配置也可以像上面一样建立多个文件的方式统一管理

全部配置完毕保存退出，然后重新启动服务器即可生效了。

#### 六、其他基本配置

`listen` 指定的就是站点端口，可以在不冲突的前提下自定义配置

`server_name` 指定域名

`index` 指定默认首页

`root` 指定网站根目录

这样基本的这些配置就能够掌握了。
