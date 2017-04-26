---
layout: post
title: eclipse安装和汉化
date: 2017-04-26
categories: blog
tags: [operations]
description: eclipse安装和汉化
---

### 下载

Eclipse 是著名的跨平台的自由集成开发环境（IDE）。最初主要用来 Java 语言开发，但是目前亦有人通过插件使其作为其他计算机语言比如 C++ 和 Python 的开发工具。

Eclipse 的本身只是一个框架平台，但是众多插件的支持，使得 Eclipse 拥有较佳的灵活性。许多软件开发商以 Eclipse 为框架开发自己的 IDE。

Eclipse 下载页面：[http://www.eclipse.org/downloads/](http://www.eclipse.org/downloads/ )

### 汉化

Eclipse 对于语言包建立了新的子项目，叫做 Babel。网址为：[http://www.eclipse.org/babel/downloads.php](http://www.eclipse.org/babel/downloads.php)

在这里你可以下载对应各个 Eclipse 版本的语言包。

单击 “ [BabelLanguagePack-eclipse-zh_4.5.0.v20151128060001.zip (87.61%)](http://www.eclipse.org/downloads/download.php?file=/technology/babel/babel_language_packs/R0.13.1/mars/BabelLanguagePack-eclipse-zh_4.5.0.v20151128060001.zip) ” 链接，下载汉化文件。

注：以上链接为 Babel Language Packs for Mars 的简体中文汉化包

### Eclipse 汉化包安装有两种方法：

#### 第一种，直接拷贝。（最简单）

将对应目录下的文件拷贝到和Eclipse对应目录下即可。

（将解压后的语言包下的features和plugins目录下的所有文件和jar包分别拷贝到Eclipse的features和plugins目录下）这样就汉化成功了，

不过这种方法日后不好管理，比如你不想用汉化的了，就要删除features和plugins目录下的和语言包相关的文件，这时估计你也不知道哪个是语言包的文件了。

#### 第二种，即links安装法。（推荐）

首先在Eclipse的目录下新建两个文件夹，命名为 links 和 eclipse_plugins （专门用来存放插件的文件夹），

再在eclipse_plugins文件下新建一个language文件夹。

最后将语言包文件解压至language文件夹下。

接下来，在links文件夹下新建一个language.link文件（后缀名无所谓，也可以取名为language.txt）

用记事本打开，在里面输入：

	path=D:\\cb\\softs\\web\\eclipse\\eclipse_plugins\\language

（这是我存放语言包的位置，要根据你的实际目录进行修改）。

注：以上文件夹名字中不要有中文，否则可能汉化失败。