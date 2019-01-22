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

## 简单工厂模式

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

就是将所有对象类又抽象出一个接口，然后在调用具体的对象工厂创建实例。

抽象工厂模式(Abstract Factory Pattern)
----------

## 1. 定义

1. 定义：就是一个超级工厂

举例理解：一辆车是一个产品，一个工厂可以生产A/B/C三种汽车，而汽车不同之处就是颜色、形状，

这样就需要汽车工厂下有形状Shape和颜色Color两个工厂。

汽车工厂的作用就是把颜色工厂和形状工厂组合起来，输出产品。

以上：汽车工厂就是抽象工厂，而形状和颜色工厂是上一节的简单工厂。

2. 应用实例：

> 服装可以看做衣服工厂、裤子工厂和鞋子工厂的组合，输出的一个个系列组合产品。

工作了，为了参加一些聚会，肯定有两套或多套衣服吧，比如说有商务装（成套，一系列具体产品）、时尚装（成套，一系列具体产品），甚至对于一个家庭来说，可能有商务女装、商务男装、时尚女装、时尚男装，这些也都是成套的，即一系列具体产品。假设一种情况（现实中是不存在的，要不然，没法进入共产主义了，但有利于说明抽象工厂模式），在您的家中，某一个衣柜（具体工厂）只能存放某一种这样的衣服（成套，一系列具体产品），每次拿这种成套的衣服时也自然要从这个衣柜中取出了。用 OO 的思想去理解，所有的衣柜（具体工厂）都是衣柜类的（抽象工厂）某一个，而每一件成套的衣服又包括具体的上衣（某一具体产品），裤子（某一具体产品），这些具体的上衣其实也都是上衣（抽象产品），具体的裤子也都是裤子（另一个抽象产品）。

3. 优点：当一个产品族中的多个对象被设计成一起工作时，它能保证客户端始终只使用同一个产品族中的对象。

4. 缺点：产品族扩展非常困难，要增加一个系列的某一产品，既要在抽象的 Creator 里加代码，又要在具体的里面加代码。

5. 使用场景： 1、QQ 换皮肤，一整套一起换。 2、生成不同操作系统的程序。

6. 注意事项：产品族难扩展，产品等级易扩展。

## 2. 举例

> 抽象工厂是一个超级工厂，就是多个工厂的一个组合，这点理解即可

下面的例子主要看类FactoryProducer， FactoryProducer调用了超级工厂，生产出了一个颜色 + 形状的组合。
这是一个工厂结合产品。

![](https://hunzino1.github.io/assets/images/2019/design_pattern/abstractfactory_pattern_uml_diagram.jpg = 830x)

- 步骤1 工厂类定义

Shape类已经在上面定义，不赘述； Color类定义

```java
// Color类
public interface Color {
   void fill();
}

// 子类Red
public class Red implements Color {

   @Override
   public void fill() {
      System.out.println("Inside Red::fill() method.");
   }
}

// 子类Green
public class Green implements Color {

   @Override
   public void fill() {
      System.out.println("Inside Green::fill() method.");
   }
}

// 子类Blue
public class Blue implements Color {

   @Override
   public void fill() {
      System.out.println("Inside Blue::fill() method.");
   }
}
```

- 步骤2 为 Color 和 Shape 对象创建抽象类来获取工厂。 AbstractFactory类

```java
public abstract class AbstractFactory {
   public abstract Color getColor(String color);
   public abstract Shape getShape(String shape) ;
}
```

- 步骤3 创建具体工厂

ShapeFactory对color接口实现返回null

```java

// ShapeFactory
public class ShapeFactory extends AbstractFactory {

   @Override
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

   @Override
   public Color getColor(String color) {
      return null;
   }
}

// ColorFactory
public class ColorFactory extends AbstractFactory {
    
   @Override
   public Shape getShape(String shapeType){
      return null;
   }
   
   @Override
   public Color getColor(String color) {
      if(color == null){
         return null;
      }        
      if(color.equalsIgnoreCase("RED")){
         return new Red();
      } else if(color.equalsIgnoreCase("GREEN")){
         return new Green();
      } else if(color.equalsIgnoreCase("BLUE")){
         return new Blue();
      }
      return null;
   }
}
```

- 步骤4 输出系列产品的传送接口 FactoryProducer （颜色 + 形状）

```java
public class FactoryProducer {
   public static AbstractFactory getFactory(String choice){
      if(choice.equalsIgnoreCase("SHAPE")){
         return new ShapeFactory();
      } else if(choice.equalsIgnoreCase("COLOR")){
         return new ColorFactory();
      }
      return null;
   }
}
```

- 步骤5 demo

```java
public class AbstractFactoryPatternDemo {
   public static void main(String[] args) {
 
      //获取形状工厂
      AbstractFactory shapeFactory = FactoryProducer.getFactory("SHAPE");
      //获取形状为 Circle 的对象
      Shape shape1 = shapeFactory.getShape("CIRCLE");
      //调用 Circle 的 draw 方法
      shape1.draw();
 
      //获取形状为 Rectangle 的对象
      Shape shape2 = shapeFactory.getShape("RECTANGLE");
      //调用 Rectangle 的 draw 方法
      shape2.draw();
      
      //获取形状为 Square 的对象
      Shape shape3 = shapeFactory.getShape("SQUARE");
      //调用 Square 的 draw 方法
      shape3.draw();
 
      //获取颜色工厂
      AbstractFactory colorFactory = FactoryProducer.getFactory("COLOR");
      //获取颜色为 Red 的对象
      Color color1 = colorFactory.getColor("RED");
      //调用 Red 的 fill 方法
      color1.fill();
 
      //获取颜色为 Green 的对象
      Color color2 = colorFactory.getColor("Green");
      //调用 Green 的 fill 方法
      color2.fill();
 
      //获取颜色为 Blue 的对象
      Color color3 = colorFactory.getColor("BLUE");
      //调用 Blue 的 fill 方法
      color3.fill();
   }
}

```
> 抽象工厂和工厂模式的区别主要在于 抽象工厂是一个超级工厂，简单工厂的产品是抽象工厂的一个属性；
