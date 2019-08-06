---
layout:     post
title:      spring入门(16) - 第五章(2) - spring AOP - AOP实战
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(16) - 第五章(2) - spring AOP - AOP实战
=======================================
这是基础系列的最后一章，原本还应该有两个章节；

spring AOP API 和 spring AspectJ

但是考虑到两点：

- 实战的基础部分，主要是实际用到的范围；
- 慕课的那个讲师讲的太垃圾，就是反反复复重复ppt，太恶心了

所以完成AOP实战之后就是勤加练习以及努力进阶了，加油。

--------------------------------------

1 基于实例分析spring AOP的使用
-------------------------------------
### 1.1 demo

[aop_demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/cheapter5_aop)

#### 1.1.1 AOP配置文件

```xml
    <!--    切面类 -->
    <bean id="moocAspect" class="com.shj.aop.schema.advice.MoocAspect"></bean>
    <!--    业务处理类 -->
    <bean id="aspectBiz" class="com.shj.aop.schema.advice.biz.AspectBiz"></bean>

    <aop:config>
        <aop:aspect id="moocAspectAop" ref="moocAspect">
            <aop:pointcut expression="execution(* com.shj.aop.schema.advice.biz.*Biz.*(..))" id="moocPiontcut"/>
            <aop:before method="before" pointcut-ref="moocPiontcut"/>
        </aop:aspect>
    </aop:config>
```

#### 1.1.2 class文件

```java
/**
 * 业务处理类
 */
public class AspectBiz {
    public void biz() {
        System.out.println(getClass().getName() + " biz(),  业务逻辑处理方法");
    }
}

/**
 * 切面方法类
 */
public class MoocAspect {
    public void before() {
        System.out.println(this.getClass().getName() + "切面前置通知： before");
    }
}

public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
        AspectBiz aspectBiz = (AspectBiz) context.getBean("aspectBiz");
        aspectBiz.biz();
    }
}
```

### 1.2 AOP的实现逻辑

> 不考虑原理,不考虑知识点，只是讲了如何去配置一个能用的AOP实例

上例是一个before类型的切面通知方法的实现。

**基于上例可以看出AOP的实现主要依靠xml配置文件，而class定义抛开xml配置的话就是单纯的一个类，里面定义了一个实例方法**

**所以学习AOP，就是看xml配置，class定义实例方法定义都可以不必关心。**

#### 1.2.1 AOP xml配置文件分析

```xml
    <bean id="moocAspect" class="com.shj.aop.schema.advice.MoocAspect"></bean>
    <bean id="aspectBiz" class="com.shj.aop.schema.advice.biz.AspectBiz"></bean>

    <aop:config>
        <aop:aspect id="moocAspectAop" ref="moocAspect">
            <aop:pointcut expression="execution(* com.shj.aop.schema.advice.biz.*Biz.*(..))" id="moocPiontcut"/>
            <aop:before method="before" pointcut-ref="moocPiontcut"/>
        </aop:aspect>
    </aop:config>
```

分析如下：

```html
(1) 两个bean标签，MoocAspect是切面类， AspectBiz是逻辑类。
        因为是before类型，所以是在执行AspectBiz逻辑方法执行之前，先执行切面方法。
        这是本项目所有要干的事，就是实现这个执行顺序。
(2) AOP的配置内容如下：
    <aop:config>
        <aop:aspect id="moocAspectAop" ref="moocAspect">
            <aop:pointcut expression="execution(* com.shj.aop.schema.advice.biz.*Biz.*(..))" id="moocPiontcut"/>
            <aop:before method="before" pointcut-ref="moocPiontcut"/>
        </aop:aspect>
    </aop:config>

    1） aop:config 就是因为AOP配置的标志。
    2） aop:aspect 定义了一个切面，这个切面方法就是引用了上面切面类MoocAspect的bean id。
                定义一个切面是前提，只有先定义一个切面，才能再去设置切面监控的方法(pointcut), 才能确定这个切面的执行策略(before/after/throw/around等)
                如下
                1） aop:pointcut 定义监控的方法，或者说哪些方法要执行这次定义的切面方法；
                    execution(* com.shj.aop.schema.advice.biz.*Biz.*(..))  是指biz包下所有Biz结尾的类，都会执行切面方法。
                2） aop:before 到这时候还没说切面方法是哪个方法呢？ before
                              所以执行策略是before策略； 然后pointcut-ref="moocPiontcut" 是关联目标类，即上面的pointcut。
```

总结完上面的配合过程，总结如下：

- 是不是切面方法全看xml AOP配置，任何一个类的任何方法都可以被指定为切面方法；
- 可以结合该实例 去理解上个博客中很多概念， 其实从作用上去理解比定义上更容易；


2 实践之后的一些概念总结
------------------------------

### 2.1 上节概念理解

```html
1) 切面 Aspect
    <aop:aspect id="moocAspectAop" ref="moocAspect">
    这一整个配置叫做一个切面吧。

2) 通知 Advice
    我的理解就是通知方法；<aop:before method="before" pointcut-ref="moocPiontcut"/>

3) 切入点 PointCut
    被切入类，即切面切入的位置；
    <aop:pointcut expression="execution(* com.shj.aop.schema.advice.biz.*Biz.*(..))" id="moocPiontcut"/>

4) 目标对象 Target Object
    本例中就是Biz类
```

### 2.2 知识拓展

#### 2.2.1 pointcut

> <aop:pointcut expression="execution(* com.shj.aop.schema.advice.biz.*Biz.*(..))" id="moocPiontcut"/>

如上，切面方法就会去找biz包下以Biz结尾的类切入， 那这就算是一种定位方法，类似的有很多，列举一下不用记：

##### 1 execution匹配

1. execution(public * *(..))   // 所有的public方法
2. execution(* set*(..))  //定位所有以set开头发方法
3. execution(* com.shj.service.AccountService.*(..))   // AccountService类的所有方法
4. execution(* com.shj.service..(..))  // service包下的所有方法
5. execution(* com.shj.service...(..))  // service包及其子包下的所有方法

##### 2 下例方法只支持spring aop

1. within(com.shj.service.*)
2. within(com.shj.service..*)  // within用于匹配指定类型内的方法执行
3. this(com.shj.service.AccountService) // this用于匹配当前AOP代理对象的执行方法
4. target() // 用于匹配当前目标对象的执行方法

#### 2.2.2 advice应用

```html
   <aop:config>
        <aop:aspect id="moocAspectAop" ref="moocAspect">
            <aop:pointcut expression="execution(* com.shj.aop.schema.advice.biz.*Biz.*(..))" id="moocPiontcut"/>

            <aop:before method="before" pointcut-ref="moocPiontcut"/>
            <aop:after method="after" pointcut-ref="moocPiontcut"/>
            <aop:after-returning method="returning" pointcut-ref="moocPiontcut"/>
            <aop:after-throwing method="throwing" pointcut-ref="moocPiontcut"/>
            <aop:around method="around" pointcut-ref="moocPiontcut" />
            <aop:around method="around" pointcut="execution(* com.shj.aop.advice.biz.AspectBiz.init(String, int) and args(bizName, times))" />
        </aop:aspect>
    </aop:config>

    如上，上例只是一个before切面，还有很多都是类似；

    注意的地方很少， after-throwing只有存在throw才生效
    此外aop:around 还支持传入参数 类型 + 形参名,如上对应java

    public class AspectBiz {
      public void init(String bizName, int times) {
       //...
      }
    }
```
