---
layout:     post
title:      python数据类型总结(对比java)
no-post-nav: true
category: ai
tags: [ai]
excerpt: 每天都不忘保持一点点紧张，直到实现目标之后。
---

> Python中常见的数据结构可以统称为容器（container）。序列（如列表和元组）、映射（如字典）以及集合（set）是三类主要的容器。
> 对于python的面向对象，len()这些方法又是将对象作为参数传递，不得不吐槽一下。

# 一 list
> list是Python内置的一种数据类型是列表，是一种有序的集合，可以随时添加和删除其中的元素。
> 对比java，list就是一种更加灵活的ArrayList。

## 对比java：

- 1 如果要取最后一个元素，除了计算索引位置外，还可以用-1做索引，直接获取最后一个元素：

``` python
    list = ['Michael', 'Bob', 'Tracy']
    list[-1]    # -- 'Tracy'

    # list[n:m] n,m是下标索引;
    list[2:3]   # ['Tracy']
    list[0:3]   # ['Michael', 'Bob', 'Tracy']

    # list[n:]  n是下标索引，从n取到最后;
    list[0:]    # ['Michael', 'Bob', 'Tracy']
    list[1:]    # ['Bob', 'Tracy']
    list[-1:]   # ['Tracy']

    # list[:m]  m是个数，从索引0开始取m个;
                m是负数时，是不取的个数；
                list[-1] 从0取至倒数第一个。

    list[:0]    # []
    list[:-1]   # ['Michael', 'Bob']
    list[:-2]   # ['Michael']
    list[:1]    # ['Michael']
    list[:2]    # ['Michael', 'Bob']
    list[:3]    # ['Michael', 'Bob', 'Tracy']

```
- 2 可以把元素插入到指定的位置，比如索引号为1的位置：

``` python
    list.insert(1, 'Jack')

```

- 3 要删除list末尾的元素，用pop()方法，删除指定位置的元素，用pop(i)方法，其中i是索引位置。

``` python
    list          # ['Michael', 'Bob', 'Tracy']
    list.pop(-1)  # 'Tracy'
    list          # ['Michael', 'Bob']
```

- 4 list里面的元素的数据类型也可以不同：

``` python
    L = ['Apple', 123, True]
```

- 5 list元素也可以是另一个list：

``` python
    s = ['python', 'java', ['asp', 'php'], 'scheme']
    len(s)      # 4
```

# 二 tuple

> tuple和list非常类似，但是tuple一旦初始化就不能修改。因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。
> 可以将tuple当作是一个final的数组。

## 注意点：

- 1 只有1个元素的tuple定义时必须加一个逗号,，来消除歧义：

``` python
    t = (1,)  # (1,)
    t = (1)   # 1

```
- 也存在“可变的”tuple：

``` python
      t = ('a', 'b', ['A', 'B'])
      t[2][0] = 'X'
      t[2][1] = 'Y'
      t             # ('a', 'b', ['X', 'Y'])
```

      类似于指针，java的jvm。
      初始定义如图：

![初始化](https://hunzino1.github.io/assets/images/2018/python_struct/init_tuple.jpg)
      当我们把list的元素'A'和'B'修改为'X'和'Y'后，tuple变为：

![change](https://hunzino1.github.io/assets/images/2018/python_struct/change.jpg)

      所以tuple本身没变，变得是list的元素，tuple一开始指向的list并没有改成别的list，
      所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向'a'，就不能改成指向'b'，
      指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！
      即“指向不变”

# 三 dict
> Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，
> 使用键-值（key-value）存储，具有极快的查找速度（按位置做索引，不会随着字典大小的增加而变慢）。

## 注意点：
- 1 通过dict提供的get()方法，如果key不存在，可以返回None，或者自己指定的value：
``` python
    d.get('Thomas')   #None
    d.get('Thomas', -1) # -1
```

- 2 请务必注意，dict内部存放的顺序和key放入的顺序是没有关系的。

### 对比list 和 dict
- dict特点：
      1、dict查找和插入的速度极快，不会随着key的增加而变慢；
      2、dict需要占用大量的内存，内存浪费多。

- list特点:
      1、查找和插入的时间随着元素的增加而增加；
      2、占用空间小，浪费内存很少。
- 总结来说：dict空间换时间，list时间换空间。

```
dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，
正确使用dict非常重要，需要牢记的第一条就是dict的key必须是不可变对象。

这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。
这个通过key计算位置的算法称为哈希算法（Hash）。

要保证hash的正确性，作为key的对象就不能变。
在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。
而list是可变的，就不能作为key。
```

# 四、set
> set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。

- 要创建一个set，需要提供一个list作为输入集合：

```python
      # 注意，传入的参数[1, 2, 3]是一个list，
      # 而显示的{1, 2, 3}只是告诉你这个set内部有1，2，3这3个元素，显示的顺序也不表示set是有序的。
      # 重复元素在set中自动被过滤。
      s = set([1, 2, 3]) #{1, 2, 3}
```

> set和dict的唯一区别仅在于没有存储对应的value，但是，set的原理和dict一样，
> 所以，同样不可以放入可变对象，因为无法判断两个可变对象是否相等，
> 也就无法保证set内部“不会有重复元素”。当把多个list放入set，会报错。
