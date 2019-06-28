---
layout:     post
title:      spring入门(008) - 第三章(3) - spring bean装配(xml配置方式) - 自动装配
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(008) - 第三章(3) - spring bean装配(xml配置方式) - 自动装配
=======================================

通过本篇博客，您可以了解到 spring bean的自动装配，即存在依赖关系的bean之间的自动注入

如有错误之处，还望指正。

-----------------------------------------

1 回顾bean的装配模式
----------------------------------------

[回忆俺的博文](https://hunzino1.github.io/spring/2019/06/13/round_1_004_spring_ioc.html)

在第二章讲注入方式时，用了以下配置

```xml
<bean id="beInjectClass" class="com.shj.pojo.BeInjectClass">、
   <!--设值注入-->
   <property name="injectClass" ref="injectClass"/>
   <!--构造注入-->
   <constructor-arg name="injectClass" ref="injectClass"/>
</bean>
```

就是如果有依赖关系，就必须用property或者constructor-arg指明依赖的bean，

这样spring才会将其注入到bean中。

```html
现在回忆起来，demo写的有点问题，
主要是没有体现出注入这个词，
实现上述配置之后，就不在需要显式的调用set或者构造器方法了，
spring会加载时自动注入
这样才能体现注入的特点。

不知道我是不是又显式地写set调用了，嘻嘻
```

**而自动装配就是property或者constructor-arg这些也省略了，不用再bean标签中注明依赖的bean了**

2 自动装配(autowiring)的实现
------------------------------------------

### 2.1 autowiring的四种实现方式

主要是在beans标签添加default-autowire属性，有四种方式(见下面的xml):

1. No: 不作任何操作
2. byName: 根据bean标签的id属性自动装配
3. byType: 根据bean标签的class属性自动装备，多个class相同会抛出异常
4. Constructor: 根据构造函数参数查找(参数类型，那原理跟byType类似)，

如果没有找到与**构造器参数类型一致**的bean会抛出异常；

总结： **都没意义，也就面试有用，实际应用肯定是用byName方式啊。**

### 2.2 byName的实现方式

[demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter3_autowiring)

如下，beans标签添加default-autowire

然后bean标签中不在需要property或者constructor-arg属性。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans default-autowire="byName">
    <bean id="autowiringBean" class="com.shj.pojo.AutowiringBean"></bean>
    <bean id="injectBean" class="com.shj.pojo.InjectBean"></bean>
</beans>
```

**依赖bean中需要写一个set方法，然后创建AutowiringBean时就会注入InjectBean实例(见main方法)**

```java
public class InjectBean {
    public void saySomething() {
        System.out.println("这是一个注入bean");
    }
}

public class AutowiringBean {
    private InjectBean injectBean;

    public void setInjectBean(InjectBean injectBean) {
        this.injectBean = injectBean;
    }

    public void say(){
        // 直接调用InjectBean的saySomething()
        this.injectBean.saySomething();
    }
}

public class Service {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
        AutowiringBean autowiringBean = (AutowiringBean) context.getBean("autowiringBean");
        autowiringBean.say();
    }

}
```
