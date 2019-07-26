---
layout:     post
title:      每日算法 - 矩阵中找一个最长上升子序列
category: better
tags: [better]
excerpt: 坚持这件小事，20190424打卡。
---


每日算法 - 矩阵中找一个最长上升子序列
=======

一、题目描述
----------

给你一个 n x m 的矩阵，求这个矩阵内部的最长上升子序列

![substring](https://hunzino1.github.io/assets/images/2019/algorithm/substring.png)

二、ruby方案
----------

记忆化深搜

将以每个点为起点的最长路径记忆。

即计算以某个点为起点计算最长路径，途径的每一个点也都是以该点为起点计算最长路径。这是记忆的使用。

而对于二维数组，直接的思路就是回溯，对一个点的上下左右进行计算。

```ruby
def max_length(matrix, rows, cols)
  return 0 if matrix.nil? || matrix.empty?

  # 最长长度
  dp = Array.new(rows) {Array.new(cols, 0)}
  visited = Array.new(rows) {Array.new(cols, false)}

  # 回溯算法一定是用下标去遍历，因为要判断范围
  #matrix.each do |array|
  #  array.each do |
  #end

  maxlength = 0
  row, col = 0, 0
  while row < rows do
    while col < cols do
      length = find_cur_node_length(matrix, rows, cols, row, col, 0, visited, dp, maxlength)
      maxlength = length if length > maxlength
      col += 1
    end
    row += 1
  end

  maxlength
end

def find_cur_node_length(matrix, rows, cols, row, col, cur_length, visited, dp, maxlength)
  if visited[row][col]
    return dp[row][col]
  end

  if col < 0 || col >= cols || row < 0 || row >= rows
    return 0
  end

  visited[row][col] = true
  length_list = []
  if row - 1 >= 0 && matrix[row][col] > matrix[row - 1][col]
    length_list << find_cur_node_length(matrix, rows, cols, row - 1, col, cur_length + 1, visited, dp, maxlength)
  end
  if col - 1 >= 0 && matrix[row][col] > matrix[row][col - 1]
    length_list << find_cur_node_length(matrix, rows, cols, row, col - 1, cur_length + 1, visited, dp, maxlength)
  end
  if row + 1 < rows && matrix[row][col] > matrix[row + 1][col]
    length_list << find_cur_node_length(matrix, rows, cols, row + 1, col, cur_length + 1, visited, dp, maxlength)
  end
  if col + 1 < cols && matrix[row][col] > matrix[row][col + 1]
    length_list << find_cur_node_length(matrix, rows, cols, row, col + 1, cur_length + 1, visited, dp, maxlength)
  end

  dp[row][col] = (length_list.empty? ? 0 : length_list.max )

  return dp[row][col]
end
```

[传送门1](https://www.cnblogs.com/ccut-ry/p/9768985.html)
[传送门2](https://www.cnblogs.com/fanmu/p/9636076.html)
