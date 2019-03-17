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

把块传到方法
--------------

块通常不在方法中定义，而是通过作为一个参数传递给块。块可以传递给任何方法，前提是方法中有**yield**关键字块才回被调用并执行。

另外，如果你的方法中有**yield**，但是调用该方法时却没有传递块的话，程序会报错，此时调用方法时必须传递块。不过，使用**block_given?** 方法，我们可以把块作为一个可选参数传递给方法。**block_given?**方法根据是否有块传递给被调用的方法返回一个布尔值。

yield 关键字同样能带参数
--------------------------

任何传递给 **yield** 关键字的参数都能作为参数传递给块。所以当块的代码被执行时，它能使用从被调用方法传过来的参数。这些参数可以是被调用方法中的局部变量。

参数的顺序需要注意，传递到块的参数顺序就是块中接收这些参数的顺序。

![](https://hunzino1.github.io/assets/images/2019/ruby_block/block2.webp)

块如何转换为 Proc 对象
------------------------

从底层看， 代码块使用分为两步：

1. 将代码打包为块
2. 调用代码块

通常在调用方法时才会定义一个块，但如果要先把代码打包起来（组织为块），以后备用呢。由于块不是对象，那么这时候就需要把Ruby 块转化为一个对象保存下来供以后调用。

为了解决这个问题， Ruby 标准库引入了 Proc 类。Proc 对象就是由块转换而来。

** inc = Proc.new { |x| x+1 }

执行块：inc.call(1) **

块就像方法的一个匿名参数。绝大多数情况下，在方法中可通过**yield**语句直接运行一个块。但是有两种情况需要使用 Proc 对象。

- 被调用方法A调用另外一个方法B，而块在方法B 中才被调用。

- 使用 Proc 的 call 方法调用块而不是 yield


基于以上两种情况，我们需要一个“块的引用”。Ruby 中使用 **&** 实现。

Ruby 允许任何对象以“块”的形式传递给方法，而 Proc 对象是“块”的引用，因为 Ruby 方法中最后一个参数有 **&** 参数时，其必须是 Proc 对象，如果不是，则会调用这个对象的**to_proc**方法把其转换为 Proc 对象，也就是块的引用。

&a 表示，a 是一个 Proc 对象，我想把它当成块来使用。看下面一个例子。

```ruby
def my_method(&block)
  puts block
  block.call
end

my_method { puts "Hello!" }

#<Proc:0x0000010124e5a8@tmp/example.rb:6>
Hello!

```

**&block**就是块的引用，block 就是 Proc 对象，通过调用 call  方法，块就被执行。这里也可使用 yield ,只不过使用 **call** 方法代码可读性更佳。

.map(&:some_method)的工作原理是什么
------------------------------------

You’ve probably used shortcuts like .map(&:capitalize) a lot, especially if you’ve done any Rails coding. It’s a very clean shortcut to .map { |title| title.capitalize }.

写了这么多年 Ruby & Rails ，写过以及遇到上面的这段代码很多吧。但是，你是否真正理解这段代码吗？ Ruby 这种漂亮的法术值得去研究一下。

基于上文讨论的 Proc 对象以及方法中的 &参数，我们举个展开讨论：

```ruby
name = ['xiaoming','laowang']
name.map(&:capitalize)
# => ["Xiaoming","Laowang"]
```

等价于

```ruby
name = ['xiaoming','laowang']
name.map { |name| name.capitalize }
# => ["Xiaoming","Laowang"]
```

这是为什么呢？
:capitalize这个 Symbol 对象在 &符之后，而 &符号 后面又必须是 Proc 对象，所以:
&:capitalize意味着向上推导出 .map(&:capitalize.to_proc)，然后 Symbol 中必定有一个 to_proc 的实例方法。而这个 to_proc 也必定调用了 capitalize方法实现了 { |name| name.capitalize }
class Symbol
  def to_proc
    Proc.new { |x| x.send(self) }
  end
end

所以如果 :capitalize调用了 Symbol to_proc 实例方法，那么则返回一个带有参数的 proc ，并且这个参数通过 send方法动态调用了 capitalize 方法（这里 self 代表调用了 to_proc 方法的的对象，也就是 :capitalize）。

总结一下：

&:capitalize 调用了 Symbol 的 to_proc 方法，返回 Proc 对象，而 proc 就是 代码块({ |name| name.capitalize }) 的引用。不得不说这实现方式真的是漂亮至极。


