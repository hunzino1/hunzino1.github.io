---
layout:     post
title:      spring入门(004) - 第二章(2) spring IOC Demo总结
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(004) - 第二章 spring IOC Demo总结
=======================================


本章专题主要包括：

1. 接口及面向接口编程
2. 什么是IOC
3. **Spring的Bean配置(xml/注解) -- 重点**
4. Bean的初始化
5. Spring的常用注入方式

本章分为两个部分，本章节会从概念和使用上介绍IOC和Bean容器；

下一章节则主要探究spring实现不同形式。

-----------------------------------------

1 面向接口编程
-----------------------------------------

就是调用接口而不直接调用方法定义，实现解耦。

没什么可说的。

2 什么是IOC
-----------------------------------------

### 2.1 概念

> 概念上的东西，理解即可

什么是IOC：程序需要一个对象实例时不是自己创建，而是向外部程序请求获取。

（其实不是向外部请求，而是外部程序【IOC容器】做的，通过配置文件主动注入）

- IOC： 控制反转
- DI： 依赖注入 (是一种技术)

也就是说，IOC的实现方法是DI

### 2.2 [IOC实现原理](https://www.yiibai.com/spring/spring_ioc_containers.html)

![container_magic](https://hunzino1.github.io/assets/images/2019/spring/container-magic.png)

如图是spring IOC的实现原理。

如图输入包括两部分，coder定义的POJO类和配置的元数据(其实就是xml或者注解以及相关的java代码)

**也就是spring容器将我们事先定义好的POJO根据元数据信息配置注入，最终输出是我们会应用需要的实例变量。

依赖的POJO_1 + 元数据 + 我们需要的POJO_2 ===> POJO_2实例**

#### 2.2.1 两种IOC容器

1 Spring BeanFactory 容器

上一章节中 core container中的bean容器

2 Spring ApplicationContext容器

上一章节中 core container中的context容器

总结： 就像上一章说的，context实现了core和bean容器，所以后者包含了前者的所有功能。

#### 2.2.2 什么被反转了？

> 控制反转，到底什么被反转了？

获取依赖对象的过程被反转了，IOC容器主动注入。

依赖对象：A中包含一个B，A依赖B。

3 Spring的Bean配置(重点)
-----------------------------------------------

Spring的Bean配置目的就是获取一个bean(实例)，并不考虑业务逻辑比如service，或者package-scan

本章节是从最基础的开始，就是撇开各种概念，单一考虑，定了一个一个POJO，然后如何获取这个bean。

### 3.1 基于XML配置的方式

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">

  <bean id="TestBeanClass" class="com.shj.ioc.interface.TestBeanClassImpl"></bean>

</beans>
```

[spring xml的文件头说明](#spring_xml)
### 3.2 基于注解的实现方式


note
---------------------------------------------------

<span id="spring_xml">**1. spring xml的文件头说明**</span>

```html
```



