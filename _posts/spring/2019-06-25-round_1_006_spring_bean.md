---
layout:     post
title:      spring入门(006) - 第三章(1) - spring bean装配(xml配置方式) - bean的配置及作用域
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(006) - 第三章(1) - spring bean装配(xml配置方式) - bean的配置及作用域
=======================================

通过本篇博客，您可以了解到

1. spring bean的配置选项
2. spring bean的作用域
3. spring bean的生命周期

如有错误之处，还望指正。

-----------------------------------------

3.1 spring bean的配置选项
-----------------------------------------

```xml
<beans>
  <bean id="beInjectClass" class="com.shj.pojo.BeInjectClass" scope="singletin">
     <!--设值注入-->
     <property name="injectClass" ref="injectClass"/>

     <!--构造注入-->
     <constructor-arg name="injectClass" ref="injectClass"/>
  </bean>
</beans>
```

如上是一个bean的xml配置，bean常见的配置项如下：

1. id： 唯一标识
2. class： 实例化哪一个类（路径）
3. scope：作用域
4. constructor arguments： 构造器参数 （constructor-arg）
5. properties：属性
6. Autowiring mode： 自动配置模式
7. lazy-initialization mode： 懒加载模式
8. Initialization/destruction method：初始化和销毁方法。

```html
以下只有class配置项是必须想。

1. id： 唯一标识
      人为的给每一个bean配置项分配一个id，唯一，重复会报错；
      所以根据id能够100%唯一确定bean实例；

2. class： 实例化哪一个类（路径）
      如上例，是bean实例的路径，也能唯一确定实例化哪个类。

3. scope：作用域
      即指定bean在IoC容器中是单例形式(singleton)或者非单例(prototype)

4. constructor arguments： 构造器参数 （constructor-arg）
      上例中的constructor-arg，指利用构造函数注入当前bean以来的bean
      就是上例中的injectClass，使用构造函数注入到beInjectClass中，再返回beInjectClass这个实例；

5. properties：属性
      和constructor-arg一样，只不过是使用set方法

constructor-arg注入类一定要有构造函数，properties一定要有set方法定义，这个很容易理解。

6. Autowiring mode： 自动配置模式
7. lazy-initialization mode： 懒加载模式
8. Initialization/destruction method：初始化和销毁方法。
```

3.2 bean的作用域scope
--------------------------------------------

> 上例中bean配置中的scope属性

scope作用域配置选项：

1. singleton: 单例
2. prototype: 非单例
3. request
4. session
5. global session

**除了1、2，后面三项都是http选项，了解即可**

```java
    ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
    BeInjectClass bean1 = (BeInjectClass) context.getBean("beInjectClass");
    BeInjectClass bean2 = (BeInjectClass) context.getBean("beInjectClass");
    ApplicationContext context1 = new ClassPathXmlApplicationContext("spring-context.xml");
    BeInjectClass bean3 = (BeInjectClass) context1.getBean("beInjectClass");
    System.out.println(bean1.hashCode() == bean2.hashCode())    //true(singleton) false(prototyp)
    System.out.println(bean1.hashCode() == bean3.hashCode())    //false
```

```html
1 singleton：单例模式，一个bean容器中只存在一份。
    什么叫一个bean容器？
      <bean id="beInjectClass" class="com.shj.pojo.BeInjectClass" scope="singletin">
      如上bean1、bean2是在context同一个容器获取的，如果beInjectClass配置的是singleton，
      此时bean1、bean2应该是同一个对象实例，即hashCode相同；
      而bean3是在另一个容器context1中获取，这样和bean1、bean2就不是在同一容器中，
      所以不受scope="singleton"的约束，是新的一个对象实例。

    其实理解了bean在每一个容器中都会注册一份，这就很容易理解了。

2 prototype:
    <bean id="beInjectClass" class="com.shj.pojo.BeInjectClass" scope="prototype">
    每次要使用（请求）一个  bean 时都会创建一个新的实例，使用完之后就会回收，所以destory不生效。
    所以上述的bean1、bean2也是不同的两个实例。

====================与 web 应用有关的作用域说明===========================

3 request：
    每次http请求 创建的实例只在当前 request内有效，新的 request请求会重新生成一份实例，同一个request请求同一份实例。

4 session： 同上。

5 global session： 基于portlet（spring web组件）的web中有效。（其他同session）
    portlet是一些系统集成（如首页、财务系统），在不同系统之间跳转时session已经发生变化了，
    所以global session可以保证这种跨session访问。

```
