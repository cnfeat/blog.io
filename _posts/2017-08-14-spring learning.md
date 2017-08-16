---
layout: post
title: spring学习笔记(一)
description:
headline:
categories: Blog
tags: spring框架 Java
coments: ture
published: ture
---
#### 开发一个spring项目

spring的层次图

![spring的层次图](http://otfc4cl9r.bkt.clouddn.com/spring%E7%9A%84%E5%B1%82%E6%AC%A1%E5%9B%BE.png)

- 引入spring的开发包(**spring-framework-4.3.10.RELEASE**)和写日志的开发包(**commons-logging-1.2.jar**) 

- 创建spring的一个核心文件applicationContext.xml，该文件放置在src目录下，该文件中引入xsd文件

- 在applicationContext.xml中配置bean

  ~~~xml
  <?xml version="1.0" encoding="utf-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
  	xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"
  	xsi:schemaLocation="
         http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
         http://www.springframework.org/schema/tx
         http://www.springframework.org/schema/tx/spring-tx-4.3.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context-4.3.xsd
         http://www.springframework.org/schema/aop
         http://www.springframework.org/schema/aop/spring-aop-4.3.xsd">
  	<!-- 在容器文件中配置bean（service/dao/domain/action/数据源 -->
  	<bean id="userService" class="com.service.UserService">
  		<!-- 注入属性的思想 -->
  		<property name="name">
  			<value>Tom</value>
  		</property>
  	</bean>
  </beans>
  ~~~

  ​

- 在test.java中使用

  ~~~java
  package com.test;

  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;

  import com.service.UserService;

  public class Test {
  	public static void main(String[] args) {
  		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
  		UserService us = (UserService) ac.getBean("userService");
  		us.sayHello();
  	}
  }
  ~~~

- 细节讨论

  传统方法和使用spring方法的区别：

  - 使用spring，没有new对象，我们把创建对象的任务交给spring框架

  - spring的运行原理图

    ![spring运行原理图](http://otfc4cl9r.bkt.clouddn.com/spring%E8%BF%90%E8%A1%8C%E5%8E%9F%E7%90%86%E5%9B%BE.png)

- 当ClassPathXmlApplicationContext("applicationContext.xml");执行时候，我们的spring容器对象被创建，同时applicationContext.xml中配置bean就会被创建（内存[hashmap/HashTable])
