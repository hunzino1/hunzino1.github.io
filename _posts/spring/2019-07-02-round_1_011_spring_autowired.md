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

```java

```
