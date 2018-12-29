---
layout:     post
title:      Ruby元编程
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---
Ruby元编程
=========

该文档列出了Ruby元编程的知识点.

阅读完该文档后，您将会了解到:

* 什么是Ruby元编程.
* Ruby元编程的主要内容概括.

--------------------------------------------------------------------------------


> Ruby元编程: 让你学会编写代码的代码

Ruby元编程知识总览
------------------
[英文原版下载](http://pan.baidu.com/s/1nt3VVQh) 密码: uc3g

INFO: 书目号: 2

- Ruby的对象模型
  + 方法 类 对象 实例变量 分别是什么
  + 如何理解ruby一切都是对象
  + 方法存在类中
  + 实例变量存在于对象中
  + 祖先链(继承 include prepend extend)
  + self
  + 继承关系和默认的祖先链(所有的对象都继承自Object， Object继承自 BasicObject 等)

- 方法
  + 方法查找链
  + 动态方法和 method_missing send define_method (**请不要在项目中使用该部分的内容**)
  + 方法定义在哪儿？如何确认一个方法的调用者
  + 方法别名，环绕别名

- 作用域
  + 变量的作用域
  + 实例变量的作用域
  + 类变量的作用域
  + 类实例变量的作用域
  + 作用域门
  + 多个语言作用域的对比(Ruby JS等)

- 常量查找链
  + [常量是如何查找的?](http://urbanautomaton.com/blog/2013/08/27/rails-autoloading-hell/)

- 单件方法
  + 如何定义类方法
  + extend
  + 单件方法对方法查找链的影响

- eval方法  (**请不要在项目中使用该部分的内容**)
  + eval
  + instance_eval
  + class_eval

- 其他
  - binding
  - 钩子方法等
