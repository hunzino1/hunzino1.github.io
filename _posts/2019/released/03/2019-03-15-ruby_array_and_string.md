---
layout:     post
title:      Ruby关于 Array 和 String 的一些实用方法
category: rubytree
tags: [rubytree]
excerpt: 到底活成什么样，说到底都是自己的选择。
---


Ruby关于 Array 和 String 的一些实用方法
=======

> 总结了一下实际项目用到的不多，但是面试中很有用的一些方法。

```ruby
```

--------------------------------------------------------------------------------

常用方法汇总
---------------------

1. [常量定义](https://www.jianshu.com/p/ca4dbe764451)

    Float::INFINITY   # 无限大
    Float::NaN        # Not a number

    float POSITIVE_INFINITY = 1.0f / 0.0f;正无穷大
    float NEGATIVE_INFINITY = -1.0f / 0.0f;负无穷大
    float NaN = 0.0f / 0.0f;表示不是一个数字
    float MAX_VALUE = 0x1.fffffeP+127f; float能表示的最大正值：3.4028235e+38f
    float MIN_NORMAL = 0x1.0p-126f; 1.17549435E-38f
    float MIN_VALUE = 0x0.000002P-126f; float能表示的最小正值 1.4e-45f
    final int MAX_EXPONENT = 127; 一个有限float数值的最大指数值
    final int MIN_EXPONENT = -126; 一个有限float数值的最小指数值
    final int SIZE = 32; 一个float类型数值占用的比特数
    final int BYTES = SIZE / Byte.SIZE; 一个float类型数值占用的字节数

2. 归并排序

```ruby
# 归并排序的思想，简而言之，将数组一分为二，分别排序后合并。
class Array
  def merge_sort!
    return self if self.size <= 1
    mid = self.size / 2
    left = self[0...mid]
    right = self[mid...self.size]

    merge(left.merge_sort!, right.merge_sort!)
  end

  def merge(left, right)
    sorted = []
    until left.empty? or right.empty?
      left.first <= right.first ? sorted << left.shift : sorted << right.shift
    end

    sorted + left + right
  end
end
```

3. 幂运算

```ruby
2 ** 2 = 4
```

4. ruby 值传递 or 引用传递

```ruby
array = Array.new(8, Array.new(8, 0))   #这种创建二维数组的方式,二维数组的引用都是同一个

##
# 只要是块传递就可以
#
array = Array.new(8) {Array.new(8, 0)}  # 快传递

require 'matrix'
m = Matrix.zero(8).to_a

Array.new(8) {[0]*8}
```

5. 读取输入

```ruby
gets

a = gets
p a
```

array
---------------

1. 多维数组降维

```ruby
array.flatten

s = [ 1, 2, 3 ] #=> [1, 2, 3]
t = [ 4, 5, 6, [7, 8] ] #=> [4, 5, 6, [7, 8]]
a = [ s, t, 9, 10 ] #=> [[1, 2, 3], [4, 5, 6, [7, 8]], 9, 10]
a.flatten #=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 只下降一维 三维降二维
a = [ 1, 2, [3, [4, 5] ] ]
a.flatten(1) #=> [1, 2, 3, [4, 5]]

a = [ 1, 2, [3, [4, 5] ] ]
a.flatten! #=> [1, 2, 3, 4, 5]
a.flatten! #=> nil

a #=> [1, 2, 3, 4, 5]
a = [ 1, 2, [3, [4, 5] ] ]
a.flatten!(1) #=> [1, 2, 3, [4, 5]]
```

2. 一维数组转换成二维数组

```ruby
["a", "b", "c", "d"].each_with_index.map{ |v, i| [v, i] }
["a", "b", "c", "d"].map.with_index.to_a
["a", "b", "c", "d"].each_with_index.to_a

arr = %w{a b c d}
arr.zip(0..arr.size)

#以上结果都是  [['a', 0], ['b', 1], ['c', 2], ['d', 3]]
```

3. array的sort方法、reverse方法、shuffle方法

```ruby
>> a.sort  #数组排序
=> [8, 17, 42]
>> a.reverse  #数组反转排序
=> [17, 8, 42]
>> a.shuffle #数组随机排序
=> [17, 42, 8]
```

4. 将整型数组转换成字符串数组

```ruby
array.map { |x| x.to_s }
```

5. array交集

解决步骤：

- 1. 先求交集

a = array1 & array2   #=> [2, 3, 4, 8]

- 2. 第一个元素被传递给块并分配给块变量：

n = 2

[2] * [array1.count(2), array2.count(2)].min

最后flat_map, 降维

注意： map 和 flat_map不同；

```ruby
array1 = [2,2,2,2,3,3,4,5,6,7,8,9]

array2 = [2,2,2,3,4,4,4,4,8,8,0,0,0]

array3 = [2,2,2,3,4,8]

array1 & array2   # [2,3,4,8], 自动去重

(array1 & array2).flat_map { |n| [n]*[array1.count(n), array2.count(n)].min } #=> [2,2,2,3,4,8]  保留重复项
```

6. 数组全排列

```ruby
class Array
  def permutations
    return [self] if size < 2
    perm = []
    each { |e| (self - [e]).permutations.each { |p| perm << ([e] + p) } }
    perm
  end
end
```

7. 其他常用操作

```ruby
1、取数

③获取一组元素range
 >>a = [ 3,9,27,4,23,9]
 >>a[0..2]
 =>[3,9,27]
 >>a[0,3] #获取索引从0开始的3个元素
 =>[3,9,27]

④根据元素获取元素索引
 查询第一个匹配元素的索引
  a.index 9 #=>1
 查询最后一个匹配元素的索引
  a.rindex 9 #=>5

⑤取出hash数组中匹配的数(只会匹配第一个)
 ee = [{id: 1,name: 'a'},{id: 2,name: 'b'}]
 ee.find{|e| e[:id] == 1} #=> {id: 1,name: 'a'}

对hash排序  sort_by
a = [{name: 'aa', t: 1},{name: 'bb', t: 3}, {name: 'cc', t: 4},{name: 'dd', t: 2}]
a.sort_by{|e| e[:t]}  #根据t升序排列
a.sort_by{|e| -e[:t]}  #根据t降序排列
```

8. stack,queue操作

```ruby
>> a.unshift 6 # 把6添加到数组前面
=> [6, 42, 8, 17, 6]
>> a.unshift(4,5) # 把4和5添加到数组前面
=> [4, 5, 6, 42, 8, 17, 6]
>> [1,2].insert(1, 'd') #将 'd' 添加到 index为1的位置，后面的元素将向后移动
=> [1, "d", 2]

# 删除数组第一个元素，并返回这个元素。如果没有则返回nil
>> a.shift       #=>1     会改变数组的值

# 删除指定索引的元素
a.delete_at( 2 )     索引无元素则返回nil，成功则返回删除的元素

# 删除匹配的元素
a.delete 'a'     #会改变数组，若要删除的元素不存在则返回nil
a.delete('b'){ 'don't have this element'}     #若要删除的元素不存在，则执行块中的内容。

d2 = [   ["January",2007],
   ["February",2007],
   ["March",2007]  ]
# 改变数组的纵横坐标
d2.transpose     #=> [["January", "February", "March"], [2007, 2007, 2007]]
```

string
-----------------

1. 字符串是否是数字

    str.to_i.to_s == str

2. 进制转换

```ruby
016进制到10进制:

>> "B0A0".hex
=> 45216
或
>> "B0A0".to_i(16)
=> 45216

10进制到16进制:
>> 45216.to_s(16)
=> "b0a0"

```
