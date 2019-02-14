---
layout: post
title: ccf csp考试相关之卖菜
date: 2015-3-02
categories: blog
tags: [ccf csp,python]
description: python参加ccf csp考试
---

python ccf题解 201809-1 卖菜  

  '''python
#卖菜
#输入
n = int(input())
a = list(map(int,input().split()))
b=[]
#计算第二天菜价
for i in range(n):
    if(i==0):
        b.append((a[0]+a[1])//2)
    elif(i==n-1):
        b.append((a[-2]+a[-1])//2)
    else:
        b.append((a[i-1]+a[i]+a[i+1])//3)
#输出
print(" ".join(map(str,b)))
'''













