---
layout:     post
title:      spring入门(004) - 第二章(2) spring 注入方式
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(004) - 第二章 spring 注入方式
=======================================

[上一章节](https://hunzino1.github.io/spring/2019/06/12/round_1_003_spring_ioc.html)中主要讲了如何借助spring获取一个bean的两种形式，

本章则是对于包含依赖关系的bean之间的配置，

之所以分开来说，是因为入门阶段的目的是为了清晰概念，而非技术细节。

注入： 说白了，就是一个bean的成员变量还是一个bean， 获取bean的一种特殊形式罢了。

Spring注入： IOC容器加载bean的时候，自动对Bean的各成员变量完成赋值（初始化）。

-----------------------------------------

1 XML配置方式
----------------------------------------

[github](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter2_ioc)

通过xml注入bean有两种方式： 设值注入 和 构造注入

### 1.1 设值注入

xml文件：

```xml
在xml中声明了两个Bean，在加载xml的时候会自动创建这两个对象。
property： 属性
      name = "beInjectClass"
           是说第一个bean中有一个名字是injectionDAO的成员变量。
      ref = "injectClass"
           是说引用的是 injectionDAO这个bean（这是第二个bean的id）

设值注入就是Spring容器自动调用set方法为第一个bean进行赋值，
所以需要beInjectClass中需要有一个setInjectClass()方法。
（这个是与平时的set一样的，只不过由spring调用了）

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="beInjectClass" class="com.shj.pojo.BeInjectClass">
        <!--设值注入-->
        <property name="injectClass" ref="injectClass"/>
    </bean>

    <bean id="injectClass" class="com.shj.pojo.InjectClass"></bean>
</beans>
```

### 1.2 构造注入

xml文件：

```xml
constructor-arg：
      在BeInjectClass中必须定义一个构造器，构造器的参数是injectClass（大小写必须与xml一致）。

这也是和平时一样，只不过是Spring调用了构造函数进行注入而已。

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="beInjectClass" class="com.shj.pojo.BeInjectClass">
        <!--构造注入-->
        <constructor-arg name="injectClass" ref="injectClass"/>
    </bean>

    <bean id="injectClass" class="com.shj.pojo.InjectClass"></bean>
</beans>
```

### 1.3 总结

```HTML
1、 上述的构造器方法或者set方法都是被注入的bean的方法；
2、 本例只是为了方便理解，其实考虑解耦，构造器和set传入的都应该是接口定义
3、 对于构造器和setter的形参名字，一定要与xml配置的名字一致，否则出错。
4、 No default constructor found;
使用设值注入或者注解的时候，如果存在构造函数，Spring要提供显式的默认构造函数，否则会出错。
```

### 都是基本很少用的用法，@Autowired可以解决绝大多数，强调目的：为了理解概念

2 基于注解的注入实现
------------------------------------------

[github](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter2_ioc_annotate)

没什么可说的，直接用Autowired注入即可，看代码

xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd 
       http://www.springframework.org/schema/context 
       http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.shj.pojo"></context:component-scan>

</beans>
```

pojo类定义

```java
@Component("injectClass")
public class InjectClass {
    private int id;
    private String name;
}

@Component("beInjectClass")
public class BeInjectClass {

    @Autowired
    private InjectClass injectClass;
    private int id;
}
```

使用

```java
   ApplicationContext context = new ClassPathXmlApplicationContext("classpath:spring-context.xml");
   BeInjectClass beInjectClass = (BeInjectClass)context.getBean("beInjectClass");
   System.out.println(beInjectClass.toString());
```

[参考博客](https://www.cnblogs.com/xiaostudy/p/9534697.html)

[spring bean装配](https://hunzino1.github.io/spring/2019/06/25/round_1_005_spring_bean.html)
