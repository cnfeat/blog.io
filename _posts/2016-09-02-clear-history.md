---
layout: post
title: CentOS清除用户登录记录和命令历史方法
date: 2016-09-02
categories: blog
tags: [centos]
description: CentOS清除用户登录记录和命令历史方法

---

清除登陆系统成功的记录

    # echo > /var/log/wtmp //此文件默认打开时乱码，可查到ip等信息
    # last //此时即查不到用户登录信息

清除登陆系统失败的记录

    # echo > /var/log/btmp //此文件默认打开时乱码，可查到登陆失败信息
    # lastb //查不到登陆失败信息
 
清除历史执行命令

    # history -c //清空历史执行命令
    # echo > ./.bash_history //或清空用户目录下的这个文件即可
 
导入空历史记录

    # vi /root/history //新建记录文件
    # history -c //清除记录 
    # history -r /root/history.txt //导入记录 
    # history //查询导入结果

example 

    # vi /root/history
    # history -c 
    # history -r /root/history.txt 
    # history 
    # echo > /var/log/wtmp  
    # last
    # echo > /var/log/btmp
    # lastb 
    # history -c 
    # echo > ./.bash_history
    # history