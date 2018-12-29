---
layout:     post
title:      Ruby知识概要
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

Ruby知识概要
==============

Ruby基础知识
------------------------------

```ruby
ruby安装，使用irb;
数字，字符串，数组，哈希，区间，正则表达式等的基本用法；
流程控制：表达式，赋值，条件语句，循环;
迭代器，block，变量作用域；
方法定义和调用；
文件读写；
类和对象：继承，对象和属性，实例方法，类方法，访问控制；
模块：命名空间，included，组合模块。
异常处理：捕获和抛出；
线程和进程；
单元测试；
Ruby和Web；
Ruby安全；
Ruby库：内置的类和模块，Ruby标准库;
ruby文档；
RubyGems进行包的管理；
常用的RubyGems包；
```

Ruby元编程
-------------------------------

```ruby
1，对象模型:继承关系；
2，方法:    方法查找链，method_missing,define_method, send,alias, alias_method;
3，代码块： 绑定的概念，binding,block,lamda,proc;
4，类定义： Class.new,Eigenclass,included,instance_eval, class_eval;
5，编写代码的代码：Kernel#eval;
6，安全元编程；
7，研读Rails源码；
8，适当使用元编程技巧，编写自己的gem包；。
```
==考虑代码可读性，不要滥用元编程。==
==学习元编程有利于读懂rails、rack等gem的源码，加深对ruby和rails的理解。==

参考资料
------------------

==ruby基础教程，programming ruby， ruby元编程，ruby官方文档，ruby组wiki。==
