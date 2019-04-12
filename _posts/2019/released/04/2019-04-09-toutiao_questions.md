---
layout:     post
title:      每日算法 -- 字典排序
category: better
tags: [better]
excerpt: 一个人有多努力，就会有多幸运。
---

字典排序
=======================================

1、字符串的字典排序集合

2、序列值(暂时不知道解决方案)

3、下一个序列

-----------------------------------

1、字符串的字典排序集合
-------------------------

思路：

  1、先全排列，再去排序

  2、使用 下一个排列的函数， 从最小值依次打印即可。

```ruby
# 去排列并不是字典序

def fun(strs)
  strs = strs.sort

  res = []
  _fun(strs, 0, res, strs.size)
  res
end

def _fun(strs, index, res, size)
  if index == size - 1
    res << strs.join
    p strs
    return
  end

  str = strs[index]
  index.upto(size - 1).each do |i|
    strs[index] = strs[i]
    strs[i] = str

    _fun(strs, index + 1, res, size)

    strs[i] = strs[index]
    strs[index] = str
  end
end
```

3、 下一个序列
------------------------------

思路：

[参考](https://blog.csdn.net/qq_33594380/article/details/82377923)

说说下一个序列，下一个序列求解的思想非常简单：

（1）从右向左找到第一个左临小于右临的位置，左临的位置标记为i

（2）从右向左找到第一个大于a[i] 的数值，位置为j

（3）交换a[i] 与a[j] 的值

（4）将i 右边的部分排成正序

[动图](https://hunzino1.github.io/assets/images/2019/better/2019_04/Next_Permutation.gif)

```ruby
def next_str(str)
  min_str = str.chars.sort.join

  size = str.size
  (size - 2).downto(0).each do |index|
    ch = str[index]
    if ch < str[index + 1]
      (size - 1).downto(index).each do |i|
        if str[i] > ch
          str[index] = str[i]
          str[i] = ch

          res = str[0..index] + str[index + 1..-1].reverse
          p res + "======" + min_str
          return
        end
      end
    end
    p min_str
  end
end
```
