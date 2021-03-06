---
layout:     post
title:      spring入门(15) - 第五章(1) - spring AOP - AOP基本概念及特点
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(15) - 第五章(1) - spring AOP - AOP基本概念及特点
=======================================

基础概念，并无代码

--------------------------------------

1 什么是AOP及实现方式
---------------------------------------

### 1.1 什么是AOP？

面向切面编程(Aspect Oriented Programming)， 实现程序功能**统一维护**的一种技术；

1. 是一种技术；
2. 可以实现统一维护某一功能， 比如记录日志，事务控制等；  都是单一的功能，统一的管理。

主要用途： 日志记录、性能统计、安全控制、事务处理、安全控制、异常处理等。

### 1.2 AOP的实现方式

两种实现方式：

1. 预编译实现； Aspect
2. 运行期动态代理方式；  JDK动态代理 或者 CGLib动态代理。 (SpringAOP、JbossAOP)

### 1.3 总结

```html
1 什么是程序功能统一维护？
  就是针对某一特定功能，比如记日志，然后将记日志这个功能集中控制，使得业务逻辑中不必考虑记日志。
  还是不太准确，因为现在项目中还是好多自己在记日志；
  记录边界日志更形象，比如方法之间的调用，我们想记录方法名/入参/出参是什么，
  除了这几个参数不一样，其他形式都一样，功能就是记录边界日志；
  那么就可以使用AOP在方法调用和返回时自动打一个日志。
  这样在正常的业务逻辑中就不用关心了。

2 两种实现方式的补充说明
  预编译实现 和 运行期动态代理方式；
  预编译就是在运行之前提前编译，用的是Aspect预编译技术；
  运行时动态代理，应该就是反射；

  Aspect本身就是比较完整的面向切面实现，是涵盖AOP API功能的，所以AOP可以使用Aspect技术实现；
  而反射技术是使用JDK还是CGLib；

  对于动态代理方式， JDK动态代理支持接口实现 和 接口集； 而如果是具体对象定义，没有实现任何接口，则使用CGLib动态代理实现AOP；

3 强调一下AOP的作用
  如下图，面向切面AOP与业务是垂直的，上面开发人员所关心的产品管理、订单管理等业务的开发，开发人员对一项业务是按照一个流程横向开发的。
  而AOP则是在一个业务开发过程中的某一个时间点切入的，到某个时间点结束，这样就与业务逻辑开发的流程垂直了。

  也就是AOP所管理的功能RD是不关心的，比如@Transactional 开启事务，连接数据库，关闭事务，这些其实我们都不关心
  我们关心的是事务包含的 CRUD 的对象操作。

```

![aop](https://hunzino1.github.io/assets/images/2019/spring/aop.png)

2 AOP的几个基本概念
---------------------

1. 切面 Aspect
2. 连接点 JoinPoint
3. 通知 Advice
4. 切入点 PointCut
5. 引入 Introduction
6. 目标对象 Target Object
7. AOP代理 AOP Proxy
8. 织入 Weaving

### 2.1 切面 Aspect

关注点，像日志、事务、验证、权限等

横切对象： 如上图，产品管理、订单管理这些都是一个个业务逻辑的处理流程，以产品管理为例，其实就是面向对象，即以产品为对象执行的一套业务流程。

而在这个整个过程中，都是被日志、事务、验证等这些关注点“监控”，可以随时在任何地方对这些对象（产品、订单。。。）进行操作。

所以日志、事务这些都是AOP操作，穿插在产品管理的处理流程中，即多次横切了产品这个对象的处理流程。

```html
关键理解点：
  1、 rd关注的业务流程，本质上都是一个个对象的执行流程；
  2、 AOP在对象执行流程中多次切入， 即记一条日志， 发起一个事务等；

  而每次切入的地方就是切面。
```

### 2.2 连接点 JoinPoint

就是某一个程序的某一条指令的地址，例如一个方法的入口处（地址的意思),程序执行过程中某个特定的点（地址）

```html
这里举一个例子，实际并不会像下面的形式一样AOP插入日志；
这个意思是说 1 和 2处都对应了寄存器中一个指令地址，而连接点就是这 某一个地址，
即是AOP的连接点是 在哪个语句后(1) 和 哪个语句前(2) 
```

```java
  System.out.println("业务执行流程");      // 1 指令地址1
  AOPUtil.logger.info("AOP切入 记录日志");
  System.out.println("业务流继续执行");    // 2 指令地址2

```

### 2.3 通知 Advice
  		
在连接点处执行关注点（AOP），执行AOP切面动作;

说的就是AOP的操作，日志、事务等；

> 下面会详细描述

### 2.4 切入点 PointCut

就是如何在程序中匹配到正确的连接点，具体的某一切面动作匹配到 特定的一个连接点。

记日志、事务操作的地方很多，哪个连接点是目前应该连接的。

### 2.5 引入 Introduction

在**不修改类代码**前提下，为类添加新的方法或者属性。（ruby的打开类，但是不改变类代码，java是通过修改字节码的方式），大多以来AspectJ或者SpringAOP的方式。

```html
这里对概念的理解都要结合AOP的主题理解。
这里应该就是指不影响业务对象结构的前提下，怎么引入切面方法；  
要么预编译 要么 动态代理。

上面的概念都是做什么的，概念是什么其实结合AOP要做什么事一想就明白了。
```

### 2.6 目标对象 Target Object

切面所通知的对象，例如下单系统中一般涉及order、customer等对象，这些业务中所有的对象都是切面可以操作的目标对象。

### 2.7 AOP代理 AOP Proxy

这个东西是AOP创建的，内部代码，不必关心。

就是执行aspect constract（也就是按照约定执行的切面动作）。 按翻译也就是在特定的地方执行某一个AOP的功能。

### 2.8 织入 Weaving

就是切面和对象（或者程序）连接起来，这就是一个AOP动作了吧, 然后创建一个被通知对象。

![concept](https://hunzino1.github.io/assets/images/2019/spring/concept.png)

3 通知 Advice 
----------------------------

> 很简单，不费劲解释了

上面简单介绍了Advice的概念, 也就是在连接点执行AOP方法；

也就是确定了在哪切入，切入方法之后, 下面在讨论切入策略；

首先三要素：

- 连接点： 即某一条寄存器地址(具体某一句java语句的位置);
- AOP方法： AOP的方法定义

最后就是切入策略了：

- 1、前置通知：
	例如连接点是一个方法，前置通知就是进入到方法之前执行的通知。
- 2、返回后通知：
	与1相对，就是方法执行之后并返回结果后，执行的通知。
- 3 抛出异常后通知：
	方法执行抛出异常之后的通知。
- 4 后通知：
	无论正常返回还是异常退出都会执行的通知。
	（2+3）
- 5 环绕通知：
	在环绕方法的内部可以执行方法（连接点），连接点前后都执行通知。

![advice](https://hunzino1.github.io/assets/images/2019/spring/advice.png)

4 AOP优势和须知
---------------------------

1. 纯java实现，不用类加载classloader， 稳定；
2. 只能支持方法定义维度使用
3. AspectJ提供全面AOP方案，而Spring AOP只是为了整合IoC，解决实际常见的问题；


