---
layout:     post
title:      每日算法 - 二叉树中的最大路径和
category: better
tags: [better]
excerpt: 坚持这件小事，20190424打卡。
---


leetcode(124) - [二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
=======

一、题目描述
----------

### 1 英文版

<div class="content__2ebE"><div html="<p>Given a <strong>non-empty</strong> binary tree, find the maximum path sum.</p>

<p>For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain <strong>at least one node</strong> and does not need to go through the root.</p>

<p><strong>Example 1:</strong></p>

<pre>
<strong>Input:</strong> [1,2,3]

       <strong>1</strong>
      <strong>/ \</strong>
     <strong>2</strong>   <strong>3</strong>

<strong>Output:</strong> 6
</pre>

<p><strong>Example 2:</strong></p>

<pre>
<strong>Input:</strong> [-10,9,20,null,null,15,7]

&amp;nbsp;  -10
&amp;nbsp; &amp;nbsp;/ \
&amp;nbsp; 9 &amp;nbsp;<strong>20</strong>
&amp;nbsp; &amp;nbsp; <strong>/ &amp;nbsp;\</strong>
&amp;nbsp; &amp;nbsp;<strong>15 &amp;nbsp; 7</strong>

<strong>Output:</strong> 42
</pre>
" class="notranslate"><p>Given a <strong>non-empty</strong> binary tree, find the maximum path sum.</p>

<p>For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain <strong>at least one node</strong> and does not need to go through the root.</p>

<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> [1,2,3]

       <strong>1</strong>
      <strong>/ \</strong>
     <strong>2</strong>   <strong>3</strong>

<strong>Output:</strong> 6
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> [-10,9,20,null,null,15,7]

&nbsp;  -10
&nbsp; &nbsp;/ \
&nbsp; 9 &nbsp;<strong>20</strong>
&nbsp; &nbsp; <strong>/ &nbsp;\</strong>
&nbsp; &nbsp;<strong>15 &nbsp; 7</strong>

<strong>Output:</strong> 42
</pre>
</div></div>

### 2 中文版

<div class="content__2ebE"><div html="<p>Given a <strong>non-empty</strong> binary tree, find the maximum path sum.</p>

<p>For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain <strong>at least one node</strong> and does not need to go through the root.</p>

<p><strong>Example 1:</strong></p>

<pre>
<strong>Input:</strong> [1,2,3]

       <strong>1</strong>
      <strong>/ \</strong>
     <strong>2</strong>   <strong>3</strong>

<strong>Output:</strong> 6
</pre>

<p><strong>Example 2:</strong></p>

<pre>
<strong>Input:</strong> [-10,9,20,null,null,15,7]

&amp;nbsp;  -10
&amp;nbsp; &amp;nbsp;/ \
&amp;nbsp; 9 &amp;nbsp;<strong>20</strong>
&amp;nbsp; &amp;nbsp; <strong>/ &amp;nbsp;\</strong>
&amp;nbsp; &amp;nbsp;<strong>15 &amp;nbsp; 7</strong>

<strong>Output:</strong> 42
</pre>
" class="notranslate"><p>Given a <strong>non-empty</strong> binary tree, find the maximum path sum.</p>

<p>For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain <strong>at least one node</strong> and does not need to go through the root.</p>

<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> [1,2,3]

       <strong>1</strong>
      <strong>/ \</strong>
     <strong>2</strong>   <strong>3</strong>

<strong>Output:</strong> 6
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> [-10,9,20,null,null,15,7]

&nbsp;  -10
&nbsp; &nbsp;/ \
&nbsp; 9 &nbsp;<strong>20</strong>
&nbsp; &nbsp; <strong>/ &nbsp;\</strong>
&nbsp; &nbsp;<strong>15 &nbsp; 7</strong>

<strong>Output:</strong> 42
</pre>
</div></div>

二、ruby方案
----------

### 1 为什么返回值是[max_left, max_right].max + root.val

[max_left, max_right].max + root.val 是当前节点为起点，子树路径的最大和；

主要是计算左子树、右子树的路径和使用，画图后很好理解，不好描述，自己画图理解；

一句话： root为起点，不能回溯。

### 2 代码

```ruby
# Definition for a binary tree node.
# class TreeNode
#     attr_accessor :val, :left, :right
#     def initialize(val)
#         @val = val
#         @left, @right = nil, nil
#     end
# end

# @param {TreeNode} root
# @return {Integer}
$res = 0

def max_path_sum(root)
  max_sum(root)
  return $res
end

def max_sum(root)
  if root.nil?
    return 0
  end

  max_left = [0, max_sum(root.left)].max
  max_right = [0, max_sum(root.right)].max
  temp = max_left + max_right + root.val
  $res = [$res, temp].max

  return [max_left, max_right].max + root.val
end
```



