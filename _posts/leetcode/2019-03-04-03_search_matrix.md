---
layout:     post
title:      leetcod(003) - 搜索二维矩阵 II
category: better
tags: [better]
excerpt: 坚持这件小事，20190311打卡。
---


[搜索二维矩阵 II](https://leetcode-cn.com/explore/interview/card/top-interview-quesitons-in-2018/261/before-you-start/1108/)
=======

一、题目描述
----------

### 1、English版本

<div class="question-description__3U1T"><div class="translation-tool__3Ffj"><span class="" data-toggle="tooltip" data-placement="left" data-original-title="显示中文" aria-hidden="true" style="cursor: pointer;"><div class="switch-base__1Zql" data-on="true"><div class="toggle__3ZBJ"></div></div></span></div><div><p>Write an efficient algorithm that searches for a value in an <i>m</i> x <i>n</i> matrix. This matrix has the following properties:</p>

<ul>
	<li>Integers in each row are sorted in ascending from left to right.</li>
	<li>Integers in each column are sorted in ascending from top to bottom.</li>
</ul>

<p><strong>Example:</strong></p>

<p>Consider the following matrix:</p>

<pre>[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
</pre>

<p>Given&nbsp;target&nbsp;=&nbsp;<code>5</code>, return&nbsp;<code>true</code>.</p>

<p>Given&nbsp;target&nbsp;=&nbsp;<code>20</code>, return&nbsp;<code>false</code>.</p>
</div></div>

### 2、 中文版

<div class="question-description__3U1T"><div class="translation-tool__3Ffj"><span class="" data-toggle="tooltip" data-placement="left" data-original-title="显示英文" aria-hidden="true" style="cursor: pointer;"><div class="switch-base__1Zql" data-on="false"><div class="toggle__3ZBJ"></div></div></span></div><div><p>编写一个高效的算法来搜索&nbsp;<em>m</em>&nbsp;x&nbsp;<em>n</em>&nbsp;矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：</p>

<ul>
	<li>每行的元素从左到右升序排列。</li>
	<li>每列的元素从上到下升序排列。</li>
</ul>

<p><strong>示例:</strong></p>

<p>现有矩阵 matrix 如下：</p>

<pre>[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
</pre>

<p>给定 target&nbsp;=&nbsp;<code>5</code>，返回&nbsp;<code>true</code>。</p>

<p>给定&nbsp;target&nbsp;=&nbsp;<code>20</code>，返回&nbsp;<code>false</code>。</p>
</div></div>

二、ruby方案
----------

> ruby一句include就可以了

```ruby
# @param {Integer[][]} matrix
# @param {Integer} target
# @return {Boolean}
def search_matrix(matrix, target)
  if matrix.empty?
    return false
  end

  matrix.each do |array|
    if array.include?(target)
      return true
    end
  end

  return false

end
```

三、python方案
-------------

> 同上

```python
```

四、java方案
----------

```java
null
```

五、经典解法，算法思想
----------

剑指offer题目，如果是静态语言并没有include?方法，只能借助题目特点。

```ruby
##
# 矩阵上下、左右有序， 可以从右上角或者左下角开始遍历；
# 以为有序，与第一个元素相比较，就可以一次去掉一行或者一列不符合的数据
# (以左下角为例)
# @param {Integer[][]} matrix
# @param {Integer} target
# @return {Boolean}
def search_matrix(matrix, target)
  if matrix.empty?
    return false
  end

  row = matrix.size - 1
  cols = matrix[0].size
  col = 0
  while row >= 0 && col < cols do
    array = matrix[row]
    if array[col] == target
      return true
    # 说明该行都大于target，向上递减
    elsif array[col] > target
      row -= 1
      next
    # 说明该列都小于target，向前递加
    else
      col += 1
    end
  end

  return false
end
```
