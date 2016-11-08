---
layout: post
title: ubuntu 下搭建 JAVA 开发环境
date: 2016-09-28
categories: blog
tags: [ubuntu]
description: ubuntu 下搭建 JAVA 开发环境

---

## 一.下载JDK

### 1. 输入命令

`java -version`

发现系统中并没有搭建java开发环境。

### 2. 搭建JAVA开发环境，第一步就是要安装JDK！

网页链接：[http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

按照自己的需求选择版本。

点击 JDK 下方的 DOWNLOAD 下载

### 3. 点击“download”后，将会进入下个页面，此处要注意：

1）选中“Accept License Agreement”

2) 根据自己的系统和需求，选择合适的版本。比如是ubuntu系统，并且是64位的，选择“LINUX X64”

## 二.安装jdk

### 1. 输入命令进行解压：

`tar -zxvf XXXXXXX`

### 2. 要对profile进行配置：

输入命令:

`vim /etc/profile`

### 3. 这一步是重中之重！

#### 1）添加JAVA_HOME路径：

`export JAVA_HOME=/xxx/xxxx/jdk1.7.0_60`

该目录是你JDK解压后的目录，比如我的解压后的目录为：

/opt/software/java/jdk1.7.0_60

所以我的路径为：

`export JAVA_HOME=/opt/software/java/jdk_1.7.0_60`

#### 2)添加JRE路径

我的为：

`export JRE_HOME=/opt/software/java/jdk_1.7.0_60/jre`

#### 3)配置CLASSPATH路径

`export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib`

#### 4）配置PATH路径

`export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin`

### 4. OK，让刚才的配置生效

输入命令：

`source /etc/profile`

### 5. 验证：

输入命令：

`java -version`

可以看到版本号，安装成功！