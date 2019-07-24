---
layout:     post
title:      spring入门(012) - 第四章(3) - spring bean装配(注解实现方式) - @Bean
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

第四章(3) - spring bean装配(注解实现方式) - @Bean
=======================================

本章主要讲解spring的@Bean注解，


--------------------------------------

1、 @Bean
---------------------------------------

> 一句话： @Bean就相当于xml中的bean标签。

### 1.1 基本应用

@Bean 可以和@component配合使用， 但是更多的是与@Configuration结合使用。

**为什么使用@Bean？ 或者说他和@Autowired有什么区别？**

首先@Bean的作用是用来获取一个bean，看上去和@Autowired没什么区别，

**但是不同的是，@bean是可以从一个普通方法中获取bean，而不是成员变量，构造器或者setter。**

#### 1 @bean形式

```java
@Configuration
public class BeanConfig {
	
	@Bean("bean")
	public MyService myService() {
		return new MyServiceImpl();
	}
}
```

#### 2 xml配置形式

```xml
<beans>
  <bean id="myService" class="com.shj.service.MyServiceImpl"/>
</beans>
```

#### 总结对比

```html
1 首先，上面两种形式是等价的，这就说明@Bean 就相当于 xml的bean标签。
    之前说@Autowired的底层实现机制实际上是使用了Spring.BeanPostProcessor处理逻辑，所以Processor不能用@Autowired获取。
    因为@Bean等价于xml bean标签，所以可以用着两种形式获取。

2 @Bean("bean")， 如果不指定name，那么name默认是函数名，即myService；

3 那么怎么@Bean呢？ 既然等价于bean标签，而bean标签的作用其实就是将bean加入到spring容器中，所以@Bean作用相同。
    所以@Bean的重点不是获取bean，而是将Bean从无到有加入到容器。

4 注意一点： 方法体内是new操作， 即创建bean

结合上例来说就是， 向IOC容器中加入了一个MyServiceImpl对象。

```

### 1.2 init 和 destroy属性

就像上面@Bean可以指定name属性一样，@Bean同样可以自定义init 和 destroy方法， 形式如下:

```java
@Configuration
public class StoreConfig {

	@Bean(name = "bean", initMethod = "init", destroyMethod = "destroy")
	public StringStore stringStore() {
		return new StringStore();
	}

}
```

在StringStore类中添加对应的方法即可。

### 1.3 demo

[demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/cheapter4_bean)


