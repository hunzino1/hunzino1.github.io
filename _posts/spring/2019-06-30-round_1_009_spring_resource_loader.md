---
layout:     post
title:      spring入门(009) - 第三章(4) - spring bean装配(xml配置方式) - Resource
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

spring入门(009) - 第三章(4) - spring bean装配(xml配置方式) - Resource
=======================================

本章是第三章的最后一站，很开心，尽管自己的计划没有完成，但是所学的东西是真的理解了

而且第四章只是注解版的重新学习，应该会很快学完。

Resource在[spring bean装配概要](https://hunzino1.github.io/spring/2019/06/25/round_1_005_spring_bean.html)中简单提到了，

本篇博客主要包括：

1. Resource和ResourceLoader介绍
2. Resource和ResourceLoader的应用举例

如有理解错误的地方，还请指正。

-----------------------------------------

1 Resource和ResourceLoader概念学习
------------------------------------------

学习之前必须要先明确基本的概念，那么spring Resource和ResourceLoader是什么，有什么作用?

### 1.1 Resource

Resource类: spring用于加载各种资源**文件**的类。

```html
总是怕自己以后犯傻比，不理解了，在这再絮叨几句。

1 Resource 就是一个类
2 这个类能干嘛，能加载文件，就好像ruby中的File类一样
3 各种文件怎么理解？

  支持uri文件，class path文件，绝对路径等等
  也就是说Resource可以的各种是指各种获取资源的方式，uri指向的资源
  class path指向的资源等，资源还是那个资源，就是访问的方式是多样的。
```

所以，Resource能支持多少种呢？

1. UrlResource: url对应的资源
2. ClassPathResource: 类路径(相对路径 idea的，Moudle Setting)下的文件
3. FileSystemResource: 绝对路径下的文件
4. ServletContextResource: web层面，访问ServletContext环境下的文件
5. InputStreamResource： InputStream类的数据
6. ByteArrayResource： ByteArray的数据

**在强调一遍，文件或者资源不是重点，这里的重点是**

**Resource适配多种获取方式**

支持用链接获取，访问相对路径获取，也可以直接拿到流数据资源

**总之，Resource的返回是一个文件信息类(Resource实例)，而获取的过程可能是**

**访问url、相对路径等多种多样的。**

### 1.2 ResourceLoader类

#### 1.2.1 ResourceLoader是干嘛的，有什么用？

上面Resource可以通过多种方式获取到资源文件，但是同时每一种方式也对应了一个实现类，

而ResourceLoader的作用就是使我们避免了直接调用各种实现类的任务

ResourceLoader有个 getResource 方法，直接返回一个Resource(文件信息类)

```html
我猜测，getResource会根据输入先判断是哪种结构，url、classPath还是其他，

然后在进行获取操作

一句话，ResourceLoader让我们只提供输入就行了，其他判断调用我们不管。
```

#### 1.2.2 ResourceLoader的使用

**首先spring的所以ApplicationContext类都默认实现了ResourceLoader接口**

**也就是说ApplicationContext可以直接调用getResource获取实例**

再看看ResourceLoader参数类型，ResourceLoader参数是前缀 + 路径的组合。

1. classpath:++++++         classpath: + 路径
2. file:+++++               file:      + 路径
3. http:+++++               http:      + 路径
4. none(无前缀)             ResourceLoader的默认实现方式是classpath

### 1.3 综合举例使用

[demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter3_resource)

在上述介绍之后很容易可以知道Resource和ResourceLoader的使用范式一定是

Resource resource = applicationContext.getResource(path);

因为applicationContext默认实现了ResourceLoader接口

```java
/**
 * 因为所有ApplicationContext都实现了ResourceLoader接口
 * 所以可以使用ApplicationContextAware获取当前context容器
 * 进而获取Resource文件信息类实例
 * @create 2019/6/30
 */
public class ResourceBean implements ApplicationContextAware {
    private ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }

    public void getResource(String path) throws IOException {
        // 获得资源文件信息
        Resource resource = this.applicationContext.getResource(path);
        System.out.println(resource.getFilename() + "++++++++++" + resource.contentLength());
    }
}

/**
 * 调用
 */
ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
ResourceBean resourceBean = (ResourceBean) context.getBean("resourceBean");
resourceBean.getResource("classpath:file/test.txt");
```
