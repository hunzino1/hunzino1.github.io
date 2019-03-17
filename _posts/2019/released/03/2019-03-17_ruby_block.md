---
layout:     post
title:      Ruby - 块
category: rubytree
tags: [rubytree]
excerpt: 到底活成什么样，说到底都是自己的选择。
---


Ruby块知识点
=======

> 博文转自 [Ruby 块从入门到精通](https://www.jianshu.com/p/356e69cd9bf9)

“块”是 Ruby 中最有用的特性之一，但是也常常被忽略。

开始学习 Ruby 块时，经常被 yield 弄的难以理解。这篇文章将会谈及这些语法概念并提供一些例子，通篇理解这篇文章，相信读者对 Ruby 块有深入的理解。

1. 入门：Ruby 块是什么
2. yield 的内部原理是什么

    为方法传递块
    
    Yield 同样能接受参数

3. 块如何转换为 Proc 对象

4. .map(&:something)的工作原理是什么

--------------------------------------------------------------------------------

入门：Ruby 块是什么
----------

Ruby 块就是放在 do 和 end 之间的代码。这就是它的全部。那么它的魔法是什么呢？

有两种方式可实现块:

1. 嵌套在 do 和 end之间多行代码
2. 被大括号包裹的一行代码

两种实现方式都是做一样的事情，如果块中的代码超过一行（这里不是指代码行数，而是显示上有换行），建议使用第一种，可读性更强。例如：

```ruby
# 多行块
ruby [1, 2, 3].each do |n|
  puts "Number #{n}"
end

# 单行块
[1, 2, 3].each {|n| puts "Number #{n}"}

```

上面的代码中，两个竖线中间的 n 是块的参数，值为每次遍历出来的数组值。运行结果如下:

``` ruby
Number 1
Number 2
Number 3
 => [1, 2, 3]
```

yield 的内部原理是什么
----------------------

yield是 Ruby块的重要概念，也是很多人疑惑的概念。在我看来，疑惑来源于yield如何调用了块和它如何传递参数给块。通过下面代码解释一下。

```ruby
def my_method
  puts "reached the top"
  yield
  puts "reached the bottom"
end

my_method do
  puts "reached yield"
end

### output
reached the top
reached yield
reached the bottom
 => nil
```

当 **my_method** 方法被调用，执行到**yield**，这时块中的代码被执行。然后，当块的代码被执行完成之后，**my_method**方法中**yield**后面的代码继续执行。

![](https://hunzino1.github.io/assets/images/2019/ruby_block/block1.webp)


