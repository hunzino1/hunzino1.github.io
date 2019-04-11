---
layout:     post
title:      leetcode所有medium题解
category: algorithm
tags: [algorithm]
excerpt: 一个人有多努力，就会有多幸运。
---

leetcode所有medium题解
=======================================

汇集了leetcode所有medium题解，主要关注点是算法思想或者堆这种比较难的数据结构。

主要是自己不熟的、不懂的，比如红黑树等。

而不是一味地在链表、栈、队列上面吃老本。

------------------------------------------------

----------------------

1、 两数相加

转成数组，然后结果放在一新数组中；

3. 无重复字符的最长子串

5. 最长回文子串
-------------------------

思路正确，回文字符串去掉首尾一定还是回文字符串， 但是递推方程不对

我的定义： f(i) 是以i为结尾的最长回文字符串， 然后找和f(i-1)的关系

但是"babab" 正确的结果[1, 1, 3, 3, 1]

当我获取 'aba' 是最大回文之后， 后一位因为只考虑前一个最大串报错，结果变为

[1, 1, 3, 1, 1]

这样后续会一直是错误的。

正确的动态规划： 在这种情况下一个参数如果不能确定结果的情况下，那就要再加一个参数限定范围。

f(i, j) 表示str[i..j] 是回文字符串，故状态转移方程

          f（i+1, j-1), str[i] == str[j]
f(i, j) = 
          false

初始状态：

dp[i][i] = 1
dp[i][i+1] = 2 if str[i] == str[i+1]

### 错误代码

```ruby
# @param {String} s
# @return {String}
# 动归，以i结尾的最长回文字符串
# 回文字符串去掉首尾一定还是回文字符串
def longest_palindrome(s)
  return "" if s.nil? || s.empty?
  
  last_max_length = [1]
  1.upto(s.size - 1).each do |index|
    ch = s[index]
    last_max = last_max_length[index - 1]
    start = index - last_max
    last_max_str = s[start...index]
    
    # 回文是相同字符
    if last_max == 1 || last_max_str.chars.uniq.join.size == 1
      if ch == s[index - 1]
        last_max_length << last_max + 1
      end
    end
      
    pre_char = s[start - 1]
    if pre_char.nil? || pre_char != ch
      last_max_length << 1
    else
      last_max_length << last_max + 2
    end
  end
  
  max = last_max_length.max
  index = last_max_length.index(max)
  start = index - max + 1
  s[start..index]
  
end
```

7 整数反转
8 字符串转整数
9 回文数

11 盛最多水的容器
---------------------

思路二：设立两个指针，一个从头一个从尾，相向而行遍历数组，每次舍弃较短边
（1）计算面积最大值的初值，该初值以数组中的第一个元素和最后一个元素构成两边。

（2）设置首尾两个指针，首指针i指向数组中的第一个元素，尾指针j指向数组中的最后一个元素。

（3）当首指针i小于尾指针j时，一直循环计算其面积。若计算所得的当前面积大于（1）步骤中所计算得的面积最大值，则更新最大值。每一次循环都舍弃索引i和索引j中较短的那一条边。

为什么每一次循环舍弃索引i和索引j中较短的那一条边，我们最终得到的结果就会是最大的面积值呢？

反证法：

假设我们现在遍历到了height数组中第i和第j个元素，且height[i] < height[j]，如果我们的面积最大值中取了第i个元素，那么构成我们的面积最大值的两个元素一定是i和j，因为j继续减小的话长方形的宽肯定一直在减小，而其高最多只能是height[i]，不可能比height[i]更大，因此我们在继续遍历的过程中，继续保持i不变而减小j是没有意义的。我们可以直接舍弃i，从i + 1开始去继续遍历。

由于整个过程只遍历了一次数组，因此时间复杂度为O(n)，其中n为数组height的长度。而使用空间就是几个变量，故空间复杂度是O(1)。

```ruby
# @param {Integer[]} height
# @return {Integer}
def max_area(height)
  start, last = 0, height.size - 1
  max = area(start, last, height)
  
  while start < last do
    area = area(start, last, height)
    max = area if area > max
    
    if height[start] < height[last]
      start += 1
    else
      last -= 1
    end
  end
  
  max
  
  
end

# 计算面积
def area(start, last, height)
  width = last - start
  high = height[start] > height[last] ? height[last] : height[start]
  high * width
end
```

14. 最长公共前缀

有意思，先去str1和str2的common， 然后用common计算

15. 三数之和
--------------------

1、先排序

2、 如果不是0、0、0， 那肯定是有正有负， 遍历负数， 每个负数用前后指针拿到另外两个数 [友情链接](https://www.jianshu.com/p/69b0a1170f96)

16. 最接近的三数之和
-----------------------

三数之和思想。

1、sort

2、 查找另外两数字

21. 合并两个有序链表
------------------------

23. 合并K个排序链表
-----------------------

[思路](https://www.cnblogs.com/grandyang/p/4606710.html)

46. 全排列
------------------

我的错误

错误原因，nums在全排列过程中已经是不用在变了，只需要一个index变量去控制即可。

```ruby
# @param {Integer[]} nums
# @return {Integer[][]}
def permute(nums)
  return [] if nums.nil? || nums.empty?
  
  res = []
  _permute(nums, [], res)
    
  res  
end
  
def _permute(nums, array, res)
  if array.size == nums.size
    res << array
    return
  end
  
  num = nums[0]
  0.upto(nums.size - 1).each do |index|
    nums[0] = nums[index]
    nums[index] = num
    array << nums[0]
    
    _permute(nums[index..-1], array, res)
    nums[index] = nums[0]
    nums[0] = num
  end
end
```

错误原因： 怀疑是引用传递的问题

```ruby
# @param {Integer[]} nums
# @return {Integer[][]}
def permute(nums)
  return [] if nums.nil? || nums.empty?
  
  res = []
  _permute(nums, 0, res)
    
  res  
end
  
def _permute(nums, index, res)
  if index == nums.size - 1
    res << nums
    return
  end
  
  num = nums[0]
  index.upto(nums.size - 1).each do |i|
    nums[0] = nums[i]
    nums[i] = num
    
    _permute(nums, index + 1, res)
    
    nums[i] = nums[0]
    nums[0] = num
  end
end
```

正确答案

```ruby
# @param {Integer[]} nums
# @return {Integer[][]}
def permute(nums)
  nums.permutation.to_a  
end

# @param {Integer[]} nums
# @return {Integer[][]}
def permute(nums)
  nums.uniq!
  return [nums] if nums.size < 2

  result = [[[], nums]]
  nums.each do
    temp = []
    result.each {|item| item.last.each {|num| temp << [item.first + [num], item.last - [num]]} }
    result = temp
  end
  result.map{|item| item.first}
end

```

59. 螺旋矩阵 II
-----------------------
[111](https://www.cnblogs.com/jimmycheng/p/7215630.html)

62. 不同路径
---------------

