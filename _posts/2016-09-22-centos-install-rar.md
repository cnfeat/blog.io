---
layout: post
title: 让 CentOS 能用 yum 自动安装 rar 和 unrar
date: 2016-09-22
categories: blog
tags: [centos]
description: 让 CentOS 能用 yum 自动安装 rar 和 unrar

---

### 如何让CentOS能用yum自动安装rar和unrar

系统环境： CentOS 7.0

### 具体操作步骤如下：

#### 1.编辑文件

编辑dag.repo文件，或者说是新建一个dag.repo文件。

`vi /etc/yum.repos.d/dag.repo`

#### 2.在文件中添加代码

该文件在我这里是个空文件，添加入下内容，然后:wq保存！

    [dag]
    name=Dag RPM Repository for Red Hat Enterprise Linux
    baseurl=http://apt.sw.be/redhat/el$releasever/en/$basearch/dag
    gpgcheck=1
    enabled=1
    gpgkey=http://dag.wieers.com/rpm/packages/RPM-GPG-KEY.dag.txt

#### 3.安装

执行下列代码自动安装rar和unrar：

`yum -y install rar unrar`

ok，大功告成！
