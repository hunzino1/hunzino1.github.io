---
layout:     post
title:      spring入门(007) - 第三章(2) - spring bean装配(xml配置方式) - aware接口
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(007) - 第三章(2) - spring bean装配(xml配置方式) - aware接口
=======================================

通过本篇博客，您可以了解到 spring bean的aware接口,主要包括

1. aware接口的描述
2. aware接口的使用

如有错误之处，还望指正。

-----------------------------------------

1 Aware接口
----------------------------------------

> 什么是Aware接口？

Aware接口是指spring提供的一些以Aware结尾的接口，这些接口的作用都是

**可以某些特定的资源，**然后可以使用该资源进行操作。

比如：ApplicationContextAware可以获得一个ApplicationContext实例；

BeanNameAware 怎可以获取一个BeanName的字符串；

### 1.1 常见的Aware接口列举：

1. ApplicationContextAware：
2. ApplicationEventPublisherAware：  事件发布
3. BeanClassLoaderAware：加载器
4. BeanFactoryAware：
5. BeanNameAware：
6. BootstrapContextAware：
7. LoadTimeWeaverAware：
8. ServletConfigAware：
9. ServletContextAware：


2 aware接口的使用
-------------------------------------------

在使用的基础上加深理解，本例包括ApplicationContextAware 和 BeanNameAware

[demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter3_aware)

> 首先先单独看一下ApplicationContextAware 和 BeanNameAware的作用

### 2.1 ApplicationContextAware 和 BeanNameAware的使用

#### 2.1.1 ApplicationContextAware： 获取一个ApplicationContext实例

Aware接口是提供某些资源，而ApplicationContextAware就是提供了一个ApplicationContext实例

**如下，setApplicationContext传入了一个applicationContext实例，由spring容器提供**

```java
public class ApplicationContextAwareBean implements ApplicationContextAware {

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
    }
}
```

#### 2.1.2 BeanNameAware： 获取一个BeanName字符串(即xml中bean的id)

```java
public class BeanNameAwareBean implements BeanNameAware {

    @Override
    public void setBeanName(String name) {
        System.out.println("beanName: " + name);
    }
}
```

#### 2.1.3 总结

由上述的例子可以隐约明白Aware的作用了，提供的资源可能是bean，也可能是字符串，也可能是其他的

### 2.2 Aware接口有什么用？

上面ApplicationContextAware可以获取一个ApplicationContext实例，BeanNameAware可以获取一个BeanName字符串

但是其他方式就可以实现，这种接口的方式有什么用？下面看一个简单的例子

**如下，可以动态配置beanNameId，然后获取不同的实例对象操作。**

即将Aware接口结合在一块，可以对多种资源协同操作。

甚至能够更改spring 容器内部信息，不过要谨慎操作。

```java
public class AwareBean implements BeanNameAware, ApplicationContextAware {

    private String beanNameId;
    private ApplicationContext applicationContext;

    @Override
    public void setBeanName(String name) {
      this.beanNameId = name;
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }

    publice void useAware() {
      // 获取bean实例去操作
      this.applicationContext.getBean(this.beanNameId);
    }
}
```

[一个简单的博客](https://www.jianshu.com/p/c5c61c31080b)
