---
layout:     post
title:      spring入门(003) - 第二章(1) spring IOC
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(003) - [第二章 spring IOC](https://www.imooc.com/video/3665)
=======================================

[github地址]()

本章专题主要包括：

1. 接口及面向接口编程
2. 什么是IOC
3. **Spring的Bean配置(xml/注解) -- 重点**
4. Bean的初始化
5. **Spring的常用注入方式 -- 重点**

本章分为两个部分，本章节会从概念和使用上介绍IOC和Bean容器；

第3节是讨论spring怎么获得一个bean，并没有涉及bean之间的注入。

而bean之间存在依赖关系时

第4节则是讨论bean之间存在依赖关系时如何实现注入，获取组装后的bean。

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

本节是从最基础的开始，就是撇开各种概念，单一考虑，定了一个一个POJO，然后如何获取这个bean。

**这一节是讨论如何获取一个bean，而将一个bean注入到另一个bean输出是第4节讨论的。**

### 3.1 基于XML配置的方式

[github地址](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter2)

配置文件如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <bean id="TestBeanClass" class="com.shj.pojo.TestBeanClass">
        <property name="name" value="shj"></property>
        <property name="id" value="19"></property>
    </bean>
</beans>
```

定义了一个POJO类TestBeanClass：

```java
public class TestBeanClass {
    private int id;
    private String name;
}
```

上个章节讲了context容器管理所有的bean，所以可以创建一个context来获取一个bean实例。

如下，元数据(xml) + POJO类(TestBeanClass) + Spring容器(context)  ===>  TestBeanClass实例

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
        TestBeanClass bean = (TestBeanClass) context.getBean("TestBeanClass");
        System.out.println(bean.getId());
    }
}
```

#### 总结几点

在程序跑起来之前，有几个地方是不懂的，虽然现在懂了，也总结在此吧。

```html
1、 xml是怎么被调用加载的？

    是不是巨傻逼的问题，不过小白对这些细节之前是真的不知道。
    ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");

    应该是spring直接查找resources下的xml配置文件，然后此时就将所有注册的bean都管理在context容器中了。

2、 是针对自己写的这个项目的一个问题，TestBeanClass的成员变量时怎么赋值的？

    首先，context的目的是获取一个bean实例，这是目的；之后直接set方法赋值即可。
    然后，如果想要有初始值，如上xml的配置
    <bean id="TestBeanClass" class="com.shj.pojo.TestBeanClass">
        <property name="name" value="shj"></property>
        <property name="id" value="19"></property>
    </bean>

    property就是bean的属性，可以通过value给bean赋上初始值；

    拓展： 在此可以推测bean质检的注入，说白了bean A注入到B，其实A就是B的一个成员变量
    那么用property应该也行。

```

[spring xml的文件头说明](#spring_xml)

### 3.2 基于注解的实现方式

4 spring bean的初始化
------------------------------------------------

note
---------------------------------------------------

<span id="spring_xml">**1. spring xml的文件头说明**</span>

```html
```

**2. 新建一个spring项目**

这次新建项目也闹出不少乌龙，虽然没必要，但事无巨细的也总结了吧。

[看这里]()


