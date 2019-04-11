---
layout:     post
title:      字典排序
category: algorithm
tags: [algorithm]
excerpt: 一个人有多努力，就会有多幸运。
---

字典排序
=======================================

1、字符串的字典排序集合

2、序列值

3、下一个序列

-----------------------------------

1、字符串的字典排序集合
-------------------------

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

3. 下一个序列
------------------------------

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
