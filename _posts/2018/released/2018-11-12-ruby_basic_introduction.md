---
layout:     post
title:      Ruby基础知识梳理
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---
Ruby基础知识梳理
=====================================

了解ruby，安装ruby和使用irb
----------------------------------------
 关于ruby（解释型语言），执行ruby文件

Ruby基础知识
-----------------------------------------
### 常用基本概念和方法
- puts, p
- 单引号''和双引号"", 字符串嵌入，“#{abc}”
- 数值表示和计算
- 注释
- 运算符及优先级

### 对象、变量和常量
- Ruby中一切皆对象
- 弱变量类型，局部变量，全局变量，实例变量，类变量
- 常量/变量命名规范，保留字
- 多重赋值，交换变量值，获取数组元素

### 条件判断
- 真假值，逻辑运算符 && || !, 不推荐使用 and or
- if, elsif, unless, case
- if/unless修饰符；

### 循环
- while, until, loop（较少使用）
- 迭代器，times, each（常用），block
- 循环控制，break, next, redo

### 方法
- 调用
- 返回值
- 实例方法
- 类方法
- 定义
- 参数, 个数不确定
- 语法糖（拟态方法）

### 类和模块
- 类和实例，继承
- class，intialize，attr_accessor, self
- 实例变量，类变量， 实例方法，类方法，常量
- public, protected, private, alias，undef
- 模块 module，命名空间，include，extend，prepend
- 祖先链 ancestors
- 方法查找规则

### 异常处理
- 异常分类
  * ScriptError
    * LoadError, SyntaxError
  * StandarError
    * RuntimeError, NameError(NoMethodError), ArgumentError, TypeError, ...

- 捕获异常 rescue
- rescue修饰符
- 抛出异常 raise
- 后处理   ensure
- 重试     retry

Ruby 常用类库
------------------------------------
 提示: 需要区分Rails的方法和Ruby原生方法

### 数值类
- Numeric
  * Integer(Fixnum, Bignum), Float, Rational（有理数）, Complex
- 数值字面量，进制，数值运算
- div，quo，modulo，divmod，remainder
- 数值类型转换
- 位运算
- 随机数，rand
- times，upto，downto，step
- Math模块， Comparable模块

### Array
-  数组的创建
   [],new，%w, %i, to_a, String#split
-  索引，[]，获取元素，赋值，values_at
-  交集&，并集|, +, -
-  常用方法
   * shift, unshift, <<, push, pop, concat, +, [],
   * compact, delete, delete_at, delete_if, reject!,
   * slice!, uniq!, collect!, map!, fill,
   * flatten!, reverse!, sort!, sort_by
   * inject, any?, all?, select, each, each_with_index, one?
-  Enumerable模块

### String
- 字符串创建
  "", '', %Q, %q, Here Document, sprintf;
- 常用方法
  * size, length, count, empty?,
  * [], +, ==, <, >,
  * split, chop!, chomp!, strip!, encode!,
  * index, include?, sub!, gsub!,
  * slice!, concat/+, delete!, reverse!,
  * upcase!, downcase!, swapcase!, capitalize!

### Symbol
- Symbol#to_s
- String#to_sym

### Hash
- 创建
  {}, Hash.new;  值获取与设定；默认值；
- 常用方法
  * keys, values, to_a, fetch;
  * key?(key), has_key?, include?, member?, value?, has_value?;
  * size, length, empty?
  * delete, delete_if, reject!
  * clear,  each;  
  * merge update
- Enumerable模块
- Hash作为方法调用的最后一个参数时，可以不加括弧 "{}"

### 正则表达式 Regexp
-  创建
   * //, Regexp.new,  %r()
-  匹配
   * =~, !~;
- match方法
-  匹配普通字符
-  行首与行尾匹配
   * ^, $,
-  字符串首部和尾部匹配
   * \A, \Z
-  指定匹配字符的范围
   []
-  匹配任意字符
   * "."
-  使用反斜杠的模式
   * \s, \d, \w, \A, \z, \（字符串转义）
-  重复
   * *, +, ?, {}, ()
-  最短匹配
   * *?, +?
-  正则表达式的选项
   * i, x, m
-  捕获
   * $1, $2, $`, $&, $'（这三个依次表示：匹配部分前面、匹配部分本身、匹配部分后面的字符串）
-  sub, gsub, scan

### File
- 继承关系
  * File.ancestors # => [File, IO, File::Constants, Enumerable, Object, Kernel, BasicObject] 
- 常用类方法
  - File.rename, File.delete, File.unlink,
  - File.open,  File.new, File.close
  - File.basename, File.dirname, File.extname,
  - File.split, File.join, File.expand_path
- File::Stat类
  - File.stat, File.ctime, File.mtime, File.atime, File.utime
  - File.chmod, File.chown
- FileTest模块
  - exist?, file?, directory?,
  - owned?, grpowned?, readable?, writable?, executable?,
  - size, size?, zero?

### 其他重要的类和模块
 - Object
 - Kernal
 - Module
 - Enumerable
 - IO
 - Encoding
 - Time
 - Date
 - Proc
 - Dir

参考书籍：《Ruby基础教程-第4版》，编号：95
