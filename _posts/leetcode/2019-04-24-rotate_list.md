---
layout:     post
title:      leetcod(061) - 旋转链表
category: better
tags: [better]
excerpt: 坚持这件小事，20190424打卡。
---


[旋转链表](https://leetcode-cn.com/problems/rotate-list/)
=======

一、题目描述
----------

### 1、English版本

<div><p>Given a linked&nbsp;list, rotate the list to the right by <em>k</em> places, where <em>k</em> is non-negative.</p>

<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> 1-&gt;2-&gt;3-&gt;4-&gt;5-&gt;NULL, k = 2
<strong>Output:</strong> 4-&gt;5-&gt;1-&gt;2-&gt;3-&gt;NULL
<strong>Explanation:</strong>
rotate 1 steps to the right: 5-&gt;1-&gt;2-&gt;3-&gt;4-&gt;NULL
rotate 2 steps to the right: 4-&gt;5-&gt;1-&gt;2-&gt;3-&gt;NULL
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> 0-&gt;1-&gt;2-&gt;NULL, k = 4
<strong>Output:</strong> <code>2-&gt;0-&gt;1-&gt;NULL</code>
<strong>Explanation:</strong>
rotate 1 steps to the right: 2-&gt;0-&gt;1-&gt;NULL
rotate 2 steps to the right: 1-&gt;2-&gt;0-&gt;NULL
rotate 3 steps to the right:&nbsp;<code>0-&gt;1-&gt;2-&gt;NULL</code>
rotate 4 steps to the right:&nbsp;<code>2-&gt;0-&gt;1-&gt;NULL</code></pre>
</div>

### 2、 中文版

<div><p>给定一个链表，旋转链表，将链表每个节点向右移动&nbsp;<em>k&nbsp;</em>个位置，其中&nbsp;<em>k&nbsp;</em>是非负数。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> 1-&gt;2-&gt;3-&gt;4-&gt;5-&gt;NULL, k = 2
<strong>输出:</strong> 4-&gt;5-&gt;1-&gt;2-&gt;3-&gt;NULL
<strong>解释:</strong>
向右旋转 1 步: 5-&gt;1-&gt;2-&gt;3-&gt;4-&gt;NULL
向右旋转 2 步: 4-&gt;5-&gt;1-&gt;2-&gt;3-&gt;NULL
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> 0-&gt;1-&gt;2-&gt;NULL, k = 4
<strong>输出:</strong> <code>2-&gt;0-&gt;1-&gt;NULL</code>
<strong>解释:</strong>
向右旋转 1 步: 2-&gt;0-&gt;1-&gt;NULL
向右旋转 2 步: 1-&gt;2-&gt;0-&gt;NULL
向右旋转 3 步:&nbsp;<code>0-&gt;1-&gt;2-&gt;NULL</code>
向右旋转 4 步:&nbsp;<code>2-&gt;0-&gt;1-&gt;NULL</code></pre>
</div>

二、ruby方案
----------

[参考](https://www.cnblogs.com/ariel-dreamland/p/9149729.html)

```ruby
# Definition for singly-linked list.
# class ListNode
#     attr_accessor :val, :next
#     def initialize(val)
#         @val = val
#         @next = nil
#     end
# end

# @param {ListNode} head
# @param {Integer} k
# @return {ListNode}
# n - k % n
def rotate_right(head, k)
  return head if head.nil?
  node = head
  len = 1
  
  while node.next do 
    node = node.next
    len += 1
  end
  node.next = head # 环
  
  steps = len - (k %len)
  step = 0
  while step < steps do
    head = head.next
    node = node.next
    step += 1
  end
  
  node.next = nil
  head
    
end
```

