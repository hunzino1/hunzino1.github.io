---
layout:     post
title:      spring入门(012) - 第四章(3) - spring bean装配(注解实现方式) - @Bean & @ImportSource @Value & @Scope
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

第四章(3) - spring bean装配(注解实现方式) - @Bean & @ImportSource @Value & @Scope
=======================================

本章主要讲解spring的@Bean注解，@ImportSource @Value注解以及@Scope注解

明白作用就好。

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


2 @ImportSource & @Value
-------------------------------------------

### 2.1 基于xml的配置文件加载过程

```xml
<beans>
  <context:property-placeholder location="classpath:com/shj/jdbc/config/jdbc.properties"/>

  <bean class="org.springframework.jdbc.datasource.DriverManagerDataSource">
      <property name="url" value="${jdbc.url}"/>
      <property name="username" value="${jdbc.username}"/>
      <property name="password" value="${jdbc.password}"/>
  </bean>
</beans>
```

如上是一个数据库驱动连接的spring xml配置用例：

1. context:property-placeholder标签的作用是加载资源文件，location属性是资源文件的路径；
2. 然后通过${key}的形式获取资源文件的内容

所以spring对资源文件的引用顺序就是： 

**context:property-placeholder先加载文件到spring；然后${key}调用资源文件**

```html
总结：
  1. properties格式的文件存储形式就是键值对；
  2. context:property-placeholder的location路径支持多种形式，注意是反斜线，不是点调用。
```

具体见下图：

![improt_source](https://hunzino1.github.io/assets/images/2019/spring/import_source.png)

### 2.2 基于@ImportSource & @Value实现资源文件的加载配置

```html
首先先明白@ImportSource & @Value是干嘛的？

  就是用来读取配置文件中的内容的。
```

#### 2.2.1 xml配置

context:property-placeholder加载资源文件还是不可缺少的。

```xml
<beans>
  <context:property-placeholder location="classpath:com/shj/jdbc/config/jdbc.properties"/>
</beans>
```

#### 2.2.2 使用注解

- @configuration，将这个类声明为一个配置文件，作为一个配置使用。
- @ImportResource 加载资源文件
- @Value引用资源文件的值进行赋值

而最后给出两个使用的场景，第一个是创建数据库连接new DriverManagerDataSource， 使用@Bean可以加载到容器中；

另一个就是一个输出验证。

所以**用@Bean配合上面两个注解，在实际场景中很实用**

总结一下使用顺序：

1. @configuration 声明类是配置文件
2. @ImportResource 加载jdbc.properties资源文件
3. @value 根据键值对赋值
4. @Bean 加载实例到容器。

注意：@ImportResource内容是xml配置文件，而不是资源文件路径

```java
@Configuration
@ImportResource("classpath:spring-context.xml")
public class JdbcConfig {
    @Value("jdbc.url")
    private String url;
    @Value("jdbc.username")
    private String username;
    @Value("jdbc.password")
    private String password;

    @Bean("dataSource")
    public DataSource getDataSource() {
        return new DriverManagerDataSource(url, username, password);
    }

    @Bean
    public MyDriveManager myDriveManager() {
        return new MyDriveManager(url, username, password);
    }
}


// ===============================================================

    @Bean("dataSource")
    public DataSource getDataSource() {
        return new DriverManagerDataSource(url, username, password);
    }

这一块和下面的xml配置是等价的

  <bean class="org.springframework.jdbc.datasource.DriverManagerDataSource">
      <property name="url" value="${jdbc.url}"/>
      <property name="username" value="${jdbc.username}"/>
      <property name="password" value="${jdbc.password}"/>
  </bean>

```

#### 2.2.3 几点注意

1. properties文件格式不带双引号， 如下图
2. 观察properties文件，返现名字是jdbc.开头，就是文件名 + 字段名的构成，这是为了避免冲突。
> 比如${username}返回的就是电脑用户名，为了防止这种冲突问题，jdbc.基本上确定了key的唯一性 

#### 2.2.4 demo

[demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/cheapter4_bean/src/main/java/com/shj/jdbc/config)
