---
layout:     post
title:      spring入门(006) - 第三章(1) - spring bean装配(xml配置方式) - bean的配置及作用域
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(006) - 第三章(1) - spring bean装配(xml配置方式) - bean的配置及作用域
=======================================

第三章和第四章都是讲spring bean装配

第三章是xml配置方式下的装配实现

第四章是基于注解的spring bean装配

然后，什么是bean装配？ 就是IOC容器管理bean，然后我们用的时候利用注入组装出存在依赖的bean。

> 说的概念再抽象，其实也只是做了一件事，就是

```html
class A {
  B b;
}

class B {}

A B这些对象就是bean，然后都是由spring容器管理，
假如我们此时需要一个A的实例，那样spring会自动组装A（将B注入到A）
这就是spring bean装配。


以上的概念再复杂也不管我们的事，我们只是操作了获取一个A的实例这一个操作。
```

-----------------------------------------

1、 内容汇总
-----------------------------------------

1. bean配置项
2. bean的作用域
3. bean的生命周期
4. bean的自动装配
5. Resource & ResourceLoader

```html
1. bean配置项
    就是xml bean标签的一些属性设置，比如id，class，scope等；

2. bean的作用域
    就是spring容器中bean是单例还是非单例配置，scope属性

3. bean的生命周期
    一个bean实例在spring容器中的状态转换；

4. bean的自动装配
    Autowired

5. Resource & ResourceLoader
    IOC加载资源文件(xml配置文件)时用到的类，也就是说用上面两个类读取xml
    当然也可以用来获取其他的资源配置信息
```


这里第三章和第四章都是按照以上的顺序来展开学习，在此总结现有一个整体认识。
