---
layout:     post
title:      spring入门(011) - 第四章(2) - spring bean装配(注解实现方式) - Autowired
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

第四章(2) - spring bean装配(注解实现方式) - Autowired
=======================================

本章主要讲解spring的@Autowired注解，这就说明

bean维度的注解定义已经讲完了，已经到了成员变量和方法参数注解维度

不太准确，还包括构造器参数

而至此，已经不涉及配置xml文件了。

--------------------------------------

1 @Required 和 @Autowired
---------------------------------------

[demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter4_autowired)

### 1.1 @Required

1. @Require只支持注解setter方法
2. 使用@Required注解时，被Required修饰的形参在配置时必须要有明确的值

```java
@Service
public class MainBean {
    private RequireBean requireBean;

    @Required
    public void setRequireBean(final RequireBean requireBean) {
        this.requireBean = requireBean;
    }
}
```
[参考链接](https://wiki.jikexueyuan.com/project/spring/annotation-based-configuration/spring-required-annotation.html)

因为Required用的不多，理解就行


### 1.2 @Autowired

1. @Autowired可以注解到成员方法/成员变量/构造器上
2. 可以通过@Autowired(required=false)允许注入的bean为null，否则会抛出异常，这种情况不要忘记判空操作

建议：将不能为空的属性使用@Required修饰

```java
@Service
public class MainBean {
    @Autowired
    private AutowiredBean autowiredBean;

    @Autowired
    public MainBean(final AutowiredBean autowiredBean) {
        this.autowiredBean = autowiredBean;
    }

    @Autowired(required = false)
    public void setAutowiredBean(final AutowiredBean autowiredBean) {
        this.autowiredBean = autowiredBean;
    }
}
```

2 @Autowired的特殊用法
-------------------------------------------

@Autowired除了可以注解到成员方法 成员变量 构造器上，还有一些特殊用法：

1. 注解spring自定义的bean
2. 注解集合类bean

### 2.1 注解spring自定义的bean

像ApplicationContext BeanFactory这些都是spring自定义的数据类型，同样可以使用@Autowired注解使用

如下，这样做的好处是不必再实现ApplicationContextAware接口，直接获取spring的相应资源。

```java
@Service
public class SpringDefineBean {
    @Autowired
    private ApplicationContext applicationContext;

    public void doSpmthing() {
        this.applicationContext.getBean("");
    }
}
```

特殊的，@Autowired的底层实现机制实际上是使用了Spring.BeanPostProcessor处理逻辑，所以

**BeanPostProcessor和我们自定义的processor是不能够使用@Autowired获取到的（本末倒置）**

那么这种bean如何获取？

可以xml配置bean标签或者使用注解@Bean获取。

（@Bean就相当于bean标签的作用，下面会介绍）

### 2.2 注解集合类bean

@Autowired可以给set、map、list类型的成员变量或者方法添加注解，这样就能获取到**所有**特定类型的bean。

如下，list类型就是获取所有CollectionBean类型的bean，

Map<String, CollectionBean> String是指bean id， CollectionBean是对象

set同理

```java
public class AutowiredCollectionBean {
    private List<CollectionBean> collectionBeanList;
    private Map<String, CollectionBean> collectionBeanMap;

    @Autowired
    public void setCollectionBeanList(final List<CollectionBean> collectionBeanList) {
        this.collectionBeanList = collectionBeanList;
    }

    @Autowired
    public void setCollectionBeanMap(final Map<String, CollectionBean> collectionBeanMap) {
        this.collectionBeanMap = collectionBeanMap;
    }
}
```

比如了list，我们希望获取到的集合bean有序，这一在bean定义上添加@Order注解或者实现org.springframework.core.Ordered接口。

3 demo
------------------------------------
