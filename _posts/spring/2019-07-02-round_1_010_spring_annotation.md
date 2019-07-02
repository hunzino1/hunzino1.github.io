---
layout:     post
title:      spring入门(010) - 第四章(1) - spring bean装配(注解实现方式) - bean的定义和作用域
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(010) - 第四章(1) - spring bean装配(注解实现方式) - bean的定义和作用域
=======================================

经过第三章的学习，对于spring的bean的装配已经有个比较清晰的认识了

虽然本章是基于注解的形式实现，但是大同小异，而且注解只能更简单

本章内容较多，主要包括：

1. classpath扫描和组件管理
2. 元注解（Meta-annotations）
3. 类的自动检测及bean的注册
4. <context:annotation-config/>
5. 类的自动检测及Bean的注册
6. 使用过滤器进行自定义扫描
7. 定义Bean
8. scope（作用域）
9. 代理模式

如理解有误，还请指正。

----------------------------------------

0 什么是bean？
---------------------------------------

经过第三章的学习，对bean的理解不再那么模糊，bean的定义是什么，其实很简单

在xml配置实现方式中，每一个bean标签就是一个bean；

而对应到注解实现形式中，其实可以单纯的任务作用在类class上的注解是一个bean

而不是还包括class中带有注解的成员变量和成员方法。

```html
对于注解形式，我理解的是

带有注解的成员变量和成员方法的参数，同样对应这一个class定义，然后也会带有注解

所以说class维度的bean已经全部包含，

但是这是在成员变量和方法参数对应的class定义一定有注解的前提，如果spring支持直接用autowired，

而对应的类不用定义，那么

注解形式的bean集合 就包括 带有注解的class + 成员变量 + 方法参数。

此处应该有实践

[demo]()
```

1 Spring 注解
----------------------------------------

### 1.1 classpath扫描和组件管理

> 该章毫无技术含量，可跳过。

引入注解主要是减少了xml的数量。

@component 是一个通用注解，可用于任何bean。

还有三个注解与@component等价，取不同的名字就是为了在不同的应用逻辑中使用：

- @Respository DAO层，持久层
- @Service 服务层
- @Controller 控制层（MVC）

其实他们都是component的实现，功能都是一样的，只是为了代码阅读效果更好。

### 1.2 元注解（Meta-annotations）

元注解: 注解的注解，元注解可以用来注解其他注解。

如下就是定义了一个service注解，而service注解继承自component注解，

继承了component的所有特性。

```java

@Component
public @interface Service {
  //....
}

```

元注解还支持定制

如下，就是限制Dog注解只能作用于方法定义之上

```java
@Target(ElementType.METHOD)
public @interface Dog {    
}
```

> 注解有很多需要学习的，可以看一下核心技术

[Java中的注解-自定义注解](https://juejin.im/post/5a619f886fb9a01c9f5b7e4f)

2 spring基于注解的bean的应用
-------------------------------------

### 2.1 注解

#### 2.1.1 什么样的注解会被注册到Spring容器中？

1. 作用在class定义上的注解 （@Component @Service @Repository @Controller）
2. 作用在成员变量或者成员方法定义（方法参数）上的注解（@Autowired）
3. 使用的是@component等四个注解，或者继承这四个注解自定义的注解

#### 2.1.2 spring容器是如何发现这些注解的？

见[类的自动检测](### 2.2 类的自动检测及Bean的注册)

### 2.2 类的自动检测及Bean的注册

对于类的自动检测，spring有两种配置方式：

1. <context:annotation-config/>
2. <context:component-scan base-package=""/>

二者的区别会在分别介绍之后，再做总结。

xml配置实现如下：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
    <context:component-scan base-package="com.shj"/>

</beans>
```

可以看出，xml的头文件添加了context约束；然后直接添加相应配置即可。

#### 2.2.1 <context:annotation-config/> 和 <context:component-scan base-package=""/> 比较

虽然二者都支持spring bean的自动扫描/注册，但是<context:annotation-config/>基本不实用

因为<context:component-scan base-package=""/>配置包含前者的全部内容。

二者的主要区别在于：

context:component-scan支持扫描类/方法/属性；

而annotation-config只支持扫描已经加入到Spring容器中的bean的方法和属性；

```html
我的理解是：

已经加入到Spring容器中的bean就是还要要用bean标签配置bean，
然后annotation-config才会自动扫描这些bean的属性和方法参数。

此处需要验证，先百度，再代码验证。
```

### 2.3 spring bean的过滤

上面<context:component-scan base-package=""/>，会将base-package下的所有带有注解的bean全部扫描注册到容器中

但是有时候只是想搜索特定的内容，或者想跳过特定的结构

此时，就需要使用到spring的过滤器配置了。

见下例，在context:component-scan标签下可以添加context:include-filter和context:exclude-filter两种过滤

- context:include-filter： 过滤搜索特定的内容，如下就是匹配正则以Stub结尾的bean
- context:exclude-filter： 过滤跳过特定的内容，如下就是跳过Repository注解注释的bean

```xml
    <context:component-scan base-package="com.shj">
        <context:include-filter type="regex" expression=".*Stub"/>
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
    </context:component-scan>
```

过滤器的type属性有5种选择：

1. annotation： 注解维度的过滤筛选
2. assignable： 特定的某一个类（不知道继承或者实现关系会不会过滤）
3. aspctj：     暂不清楚
4. regex：      基于正则表达式的筛选
5. custom：     暂不清楚

[参考地址]()

还可以使用use-default-filters属性禁止自动发现某个包下的bean

<context:component-scan base-package="com.shj" use-default-filters="false"/>

3 bean的定义和scope作用域
---------------------------------------------

xml中基于配置的实现已经熟知，此处只不过变成了注解，bean的定义使用@Component等四个注解，

而scope也不再是bean标签的一个属性，而是@Scope注解，理解本质即可。

### 3.1 bean的定义

1. 形如下文，@Component等四个注解作用在class定义上，就是定义了一个bean
2. @Component/@Service这些注解都有一个默认的name属性，相当于bean标签的id；
3. 没有指定name属性的，默认name为class首字母小写

```java
@Service("annotationBean")
public class AnnotationBean {
    @Autowired
    private MemberBean memberBean;
}

@Component
public class MemberBean {
}
```

#### 3.1.1 自定义bean name的命名规则(2个步骤)

> 主要是实现BeanNameGenerator接口

1 xml配置自定义命名属性generator

```xml
    <context:component-scan base-package="com.shj" name-generator="com.shj.pojo.MyNameGenerator" />
```

2 实现BeanNameGenerator接口

**注意： 一定要包含一个无参构造器**

```java
public class MyNameGenerator implements BeanNameGenerator {
    @Override
    public String generateBeanName(BeanDefinition beanDefinition, BeanDefinitionRegistry beanDefinitionRegistry) {
        return null;
    }
}
```

如此，扫描base-package="com.shj"下的bean时，没有制定name的bean会全部按照自定义的规则生成name默认值

### 3.2 scope

java和Spring都支持注解的叠加，spring bean默认是单例模式，可以使用@Scope更改

```java
@Scope("prototype")
@Component
public class ScopeBean {
}
```

#### 3.2.1 scope自定义作用域范围（2个步骤）

```html
首先说一下自定义作用域范围的好处是什么？

sigleton是在一个容器中一直使用的是一个实例bean；而prototype则是每次执行getBean是返回一个新的实例。

不过有时会有一些特殊的需求，比如在一个线程池中，想实现每一个线程对应一个实例，线程间不同

此时就需要自定义一下scope的作用域实现了。
```

1 xml配置

```xml
    <context:component-scan base-package="com.shj" scope-resolver="com.shj.pojo.ScopeDefineBean" />
```

2 实现ScopeMetadataResolver接口

**注意： 同样要求有一个无参构造器**

```java
public class ScopeDefineBean implements ScopeMetadataResolver {
    @Override
    public ScopeMetadata resolveScopeMetadata(BeanDefinition beanDefinition) {
        return null;
    }
}
```

### 3.3 代理模式 scoped-proxy

[参考一](https://www.cnblogs.com/fightingcoding/p/component-scan.html)

[参考二](http://www.bubuko.com/infodetail-1434289.html)

scope代理，有三个值选项，no(默认值)，interfaces(接口代理)，targetClass（类代理）.

那什么时候需要用到scope代理呢?

```html
  举个例子，我们知道Bean的作用域scope有singleton，prototype，request,session,

  那有这么一种情况，当你把一个session或者request的Bean注入到singleton的Bean中时，

  因为singleton的Bean在容器启动时就会创建A，而session的Bean在用户访问时才会创建B，

  那么当A中要注入B时，有可能B还未创建，这个时候就会出问题，那么代理的时候来了，

  B如果是个接口，就用interfaces代理，是个类则用targetClass代理。
```

使用：

```xml
  <context:component-scan base-package="com.shj" scoped-proxy="no" />
```

scoped-proxy和scope-resolver只能二选一

### 3.4 总结，以上说的所有配置都是context:component-scan的一个属性发现没有

以上说的所有配置都是context:component-scan的一个属性发现没有，

所以在此总结一下，加深印象

```xml
<context:component-scan base-package="com.wjx.betalot" <!-- 扫描的基本包路径 -->
                        annotation-config="true" <!-- 是否激活属性注入注解 -->
                        name-generator="org.springframework.context.annotation.AnnotationBeanNameGenerator"  <!-- Bean的ID策略生成器 -->
                        resource-pattern="**/*.class" <!-- 对资源进行筛选的正则表达式，这边是个大的范畴，具体细分在include-filter与exclude-filter中进行 -->
                        scope-resolver="org.springframework.context.annotation.AnnotationScopeMetadataResolver" <!-- scope解析器 ，与scoped-proxy只能同时配置一个 -->
                        scoped-proxy="no" <!-- scope代理，与scope-resolver只能同时配置一个 -->
                        use-default-filters="false" <!-- 是否使用默认的过滤器，默认值true -->
                                  >
            <!-- 注意：若使用include-filter去定制扫描内容，要在use-default-filters="false"的情况下，不然会“失效”，被默认的过滤机制所覆盖 -->                   
            <!-- annotation是对注解进行扫描 -->
            <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/> 
            <!-- assignable是对类或接口进行扫描 -->
            <context:include-filter type="assignable" expression="com.wjx.betalot.performer.Performer"/>
            <context:include-filter type="assignable" expression="com.wjx.betalot.performer.impl.Sonnet"/>
            
            <!-- 注意：在use-default-filters="false"的情况下，exclude-filter是针对include-filter里的内容进行排除 -->
            <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
            <context:exclude-filter type="assignable" expression="com.wjx.betalot.performer.impl.RainPoem"/>
            <context:exclude-filter type="regex" expression=".service.*"/> 

</context:component-scan>
```

### 3.5 demo

[demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter4_annotation)

```java
public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
        AnnotationBean annotationBean = (AnnotationBean) context.getBean("annotationBean");
        System.out.println(annotationBean.hashCode());
}
```

总结：

1. 注解ApplicationContext也不能丢，因为要引入xml文件

这是我犯了迷糊的地方，还是得多多联系，懂了到熟了。
