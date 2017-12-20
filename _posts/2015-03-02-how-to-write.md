---
layout: post
title: aJava基本类型和引用类型
date: 2015-3-02
categories: blog
tags: [基本类型、引用类型]
description: 文章金句。
---

8种基本类型 

一、4种整型 
    byte      1字节           -128——127 

    short     2 字节         -32,768 —— 32,767 

    int       4 字节          -2,147,483,648 ——2,147,483,647（超过20亿） 

    long      8 字节   -9,223,372,036,854,775,808——9,223,372,036854,775,807 

    注释：java中所有的数据类所占据的字节数量与平台无关，java也没有任何无符号类型 

二、 2种浮点类型 
    float    4 字节         32位IEEE 754单精度（有效位数 6 – 7位） 

    double   8 字节         64位IEEE 754双精度（有效位数15位） 

三、1种Unicode编码的字符单元 
    char    2 字节          整个Unicode字符集 

四、1种真值类型 
boolean    1 位             True或者false 
 

3种引用类型 

类class 

接口interface 

数组array 

一、类Class引用 
可以是我们创建的，这里我不多讲，主要是讲解几个java库中的类 

Object ：Object是一个很重要的类，Object是类层次结构的根类，每个类都使用Object作为超类，所有对象（包括数 

               组）都实现这个类的方法。用Object可以定义所有的类 

              如： 

              Object object= new Integer(1); 来定义一个Interger类 

               Integer i=(Integer) object;     在来把这个Object强制转换成Interger类 

String ：String类代表字符串，Java 程序中的所有字符串字面值（如"abc"）都作为此类的实例来实现。检查序列的单 

               个字符、比较字符串、搜索字符串、提取子字符串、创建字符串副本、在该副本中、所有的字符都被转换为                      大 写或小写形式。 

Date ：Date表示特定的瞬间，精确到毫秒。Date的类一般现在都被Calendar 和GregorianCalendar所有代替 

Void ：Void 类是一个不可实例化的占位符类，它保持一个对代表 Java 关键字 void 的 Class 对象的引用。 

同时也有对应的Class如：Integer  Long  Boolean  Byte  Character  Double  Float   Short 

二、接口interface引用 
可以是我们创建的，这里我不多讲，主要是讲解几个java库中的接口interface 

List<E>：列表 ，此接口的用户可以对列表中每个元素的插入位置进行精确地控制。用户可以根据元素的整数索引 

（在列表中的位置）访问元素，并搜索列表中的元素。List 接口提供了两种搜索指定对象的方法。从 

性能的观点来看，应该小心使用这些方法。在很多实现中，它们将执行高开销的线性搜索。 List 接 

口提供了两   种在列表的任意位置高效插入和移除多个元素的方法。 

add() : 在列表的插入指定元素。 

remove()：移除列表中指定位置的元素。 

             get(int index)：返回列表中指定位置的元素。 



Map<K,V>： 

K - 此映射所维护的键的类型 

V - 映射值的类型 将键映射到值的对象。一个映射不能包含重复的键；每个键最多只能映射到一个值。 

这里我们主要是用String List Map Object 是最常用Number ArrayList<E> Arrays等 

可以查考jdk的api 

这些类和接口在 

java.lang ：提供利用 Java 编程语言进行程序设计的基础类。 











