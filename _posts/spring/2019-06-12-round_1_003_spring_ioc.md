---
layout:     post
title:      spring入门(003) - 第二章(1) spring IOC
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(003) - [第二章 spring IOC](https://www.imooc.com/video/3665)
=======================================

本章专题主要包括：

1. 接口及面向接口编程
2. 什么是IOC
3. **Spring的Bean配置(xml/注解) -- 重点**
4. Bean的初始化

本章分为两个部分，本章节会从概念和使用上介绍IOC和Bean容器；

第3节是讨论spring怎么获得一个bean，并没有涉及bean之间的注入。

而bean之间存在依赖关系时则放在下一章节讨论，

目的是这一节单纯学好怎么去获取一个bean，

而下一节是学习获取一个注入的bean，

之所以分开来说，是因为入门阶段的目的是为了清晰概念，而非技术细节。

即清晰的知道利用spring获取一个bean的两种实现方式(xml/注解)

利用spring获取带有bean依赖的实现形式。

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

**也就是spring容器将我们事先定义好的POJO根据元数据信息配置注入，最终输出是我们会应用需要的实例变量。**

**依赖的POJO_1 + 元数据 + 我们需要的POJO_2 ===> POJO_2实例**

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

[参考](https://www.cnblogs.com/qinjf/p/9052445.html)

[github地址](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter2_annotate)

配置文件如下：

和xml中bean配置不同，注解方式会使用context:component-scan将带有注解标志的类、方法、属性等加载到容器中。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 开启注解的扫描。到配置的包里面扫描类、方法、属性上面是否有注解 -->
    <context:component-scan base-package="com.shj.pojo"></context:component-scan>

</beans>
```

定义了一个POJO类TestBeanClass：
spring会将带有注解的类加载到容器中

```java
@Component("user")
public class User {
    private int id;
    private String name;
}
```
调用，直接调用对应的bean即可

```java
    ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
    User user = (User) context.getBean("user");
```

4 spring bean的初始化
------------------------------------------------

什么是bean的初始化？ 就是spring容器实例化一个bean。

根据上一章节所述，实例化bean主要是bean容器和context容器：

- org.springframework.beans:  有一个BeanFactory类，加载初始化bean（对象实例）。
- org.springframework.context:    有一个ApplicationContext类，用于获取bean对象(实例对象)。

5 note
---------------------------------------------------

### <span id="spring_xml">5.1 spring xml的文件头说明</span>

```html
```

### 5.2. 新建一个spring项目

这次新建项目也闹出不少乌龙，虽然没必要，但事无巨细的也总结了吧。

[看这里](https://hunzino1.github.io/spring/2019/06/13/create_spring_project.html)

### 5.3 ApplicationContext的三种xml获取方式

1. 本地文件
2. Classpath
3. web应用中依赖servlet或listener。

![xml](https://hunzino1.github.io/assets/images/2019/spring/xml.png)

```java
  new ClassPathXmlApplicationContext("classpath:spring-context.xml")
  classpath:是什么鬼？

  1、冒号后面不能留空格
  2、classpath:应该会被截取，然后读取到spring-context.xml文件
      具体细节在进阶阶段可以好好解读
```

### 5.4 单元测试

> 单元测试很重要的，所以有必要总结一下, 不过使用单元测试套路很单一

以chapter2_annotate中使用单元测试为例：

```html
    1、 引入junit包
    2、 创建一个公共类UnitTestBase类，包含的就两点点：加载XML创建容器(before)、销毁容器(after)
    3、 使用时测试类继承UnitTestBase， 直接传入xml后，super.getBean()即可获取实例类
```

详细信息见代码

![junit](https://hunzino1.github.io/assets/images/2019/spring/junit.png)

