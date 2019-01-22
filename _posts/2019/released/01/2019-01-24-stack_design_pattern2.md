---
layout:     post
title:      技术栈3 - Design Pattern(2) - 创建型模式
category: stack
tags: [stack]
excerpt: 
---

创建型模式
=============

该文档简单介绍了设计模式中的创建型模式，阅读完该文档后，您将会了解到:

* 工厂模式（Factory Pattern）
* 抽象工厂模式（Abstract Factory Pattern）
* 单例模式（Singleton Pattern）
* 建造者模式（Builder Pattern）
* 原型模式（Prototype Pattern）

这篇博客主要是对常用模式着重总结，后两种会简单介绍。

--------------------------------------------------------------------------------

1. 工厂模式
----------

## 1. 简单工厂模式

### 1.定义

1. 主要解决问题： 一个抽象类下的多个子类，通过调用工厂接口，可以创建出指定的子类。

2. 优点： 1、一个调用者想创建一个对象，只要知道其名称就可以了。 2、扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。 3、屏蔽产品的具体实现，调用者只关心产品的接口。

3. 缺点：每次增加一个产品时，都需要增加一个具体类和对象实现工厂，使得系统中类的个数成倍增加，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。这并不是什么好事。

4. 使用场景： 1、日志记录器：记录可能记录到本地硬盘、系统事件、远程服务器等，用户可以选择记录日志到什么地方。 2、数据库访问，当用户不知道最后系统采用哪一类数据库，以及数据库可能有变化时。 3、设计一个连接服务器的框架，需要三个协议，"POP3"、"IMAP"、"HTTP"，可以把这三个作为产品类，共同实现一个接口。

5. 注意事项：作为一种创建类模式，在任何需要生成复杂对象的地方，都可以使用工厂方法模式。有一点需要注意的地方就是复杂对象适合使用工厂模式，而简单对象，特别是只需要通过 new 就可以完成创建的对象，无需使用工厂模式。如果使用工厂模式，就需要引入一个工厂类，会增加系统的复杂度。

### 2.实例

如下图， 接口Shape下实现了三个子类， ShapeFactory是创建对象的工厂.
简单来说就是ShapeFactory有一个getShape方法，可以根据类名返回对应的子类实例。

可以总结几点：

1. 工厂类： 三个子类就是所说的工厂类；
2. 如代码所示，创建子类只需要传入类名即可，有点反射的意思；
3. 每次增加一个产品时，是说没有引入的父类产品，就要新增一个工厂接口，同时定义一个工厂类。

![](https://hunzino1.github.io/assets/images/2019/design_pattern/factory_pattern_uml_diagram.jpg)

- 步骤1：类定义

```java
// Shape.java
public interface Shape {
   void draw();
}

// 子类Rectangle
public class Rectangle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}

//子类Square
public class Square implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Square::draw() method.");
   }
}

//子类Circle
public class Circle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Circle::draw() method.");
   }
}
```

- 步骤2 工厂创建

```java
public class ShapeFactory {
    
   //使用 getShape 方法获取形状类型的对象
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }        
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      return null;
   }
}
```

- 步骤3 创建demo

```java
public class FactoryPatternDemo {
 
   public static void main(String[] args) {
      ShapeFactory shapeFactory = new ShapeFactory();
 
      //获取 Circle 的对象，并调用它的 draw 方法
      Shape shape1 = shapeFactory.getShape("CIRCLE");
      //调用 Circle 的 draw 方法
      shape1.draw();
 
      //获取 Rectangle 的对象，并调用它的 draw 方法
      Shape shape2 = shapeFactory.getShape("RECTANGLE");
      //调用 Rectangle 的 draw 方法
      shape2.draw();
 
      //获取 Square 的对象，并调用它的 draw 方法
      Shape shape3 = shapeFactory.getShape("SQUARE");
      //调用 Square 的 draw 方法
      shape3.draw();
   }
}

```

## 2.工厂方法（简单工厂扩展）

> 工厂方法，简单来说就是将工厂又抽象出一个接口

![](https://hunzino1.github.io/assets/images/2019/design_pattern/factory.png)



章节2
----------

