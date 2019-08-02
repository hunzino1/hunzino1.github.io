---
layout:     post
title:      spring入门(013) - 第四章(4) - spring bean装配(注解实现方式) - 基于泛型的自动装配
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

第四章(4) - spring bean装配(注解实现方式) - 基于泛型的自动装配
=======================================

就是 泛型 + @Autowired， 只是在以后实践中需要多理解泛型的实际使用意义，

这样才能更好地理解本章节的作用

本章节基于实例进行讲述，并不会太详细，因为不是在理解的基础上，意义不大。

--------------------------------------

1 使用说明
-------------------------------------

主要就是包含几个要素：

1. @Autowired实现自动装配
2. 使用泛型

而下例中的@Bean注解纯粹是为了巩固上节的内容

2 demo
------------------------------------

[demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/cheapter4_bean/src/main/java/com/shj/generic)

1. @Configuration 也会将实例注册到容器中
2. 如下，直接用@Autowired修饰泛型即可
3. @Bean在这没必要，demo中用了@Service同样可以；

```java
@Configuration
public class StoreConfig {
    @Autowired
    private Store<Integer> integerStore;
    @Autowired
    private Store<String> stringStore;

    @Bean
    public Store integerStore() {
        return new IntegerStore();
    }

    /**
     * 注释之后
     * string也默认走了Integer，名称找不到估计按类型匹配了
     */
//    @Bean
//    public Store stringStore() {
//        return new StringStore();
//    }

    public void testGeneric() {
        System.out.println("integerStore: " + integerStore.getClass().getName());
        System.out.println("stringStore: " + stringStore.getClass().getName());
    }
}
```


