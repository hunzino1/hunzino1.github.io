---
layout:     post
title:      leetcod(004) - 合并两个有序数组
category: algorithm
tags: [algorithm]
excerpt: 坚持这件小事，20190311打卡。
---


[合并两个有序数组](https://leetcode-cn.com/explore/interview/card/top-interview-quesitons-in-2018/261/before-you-start/1109/)
=======

一、题目描述
----------

### 1、English版本

<div class="question-description__3U1T"><div class="translation-tool__3Ffj"><span class="" data-toggle="tooltip" data-placement="left" data-original-title="显示中文" aria-hidden="true" style="cursor: pointer;"><div class="switch-base__1Zql" data-on="true"><div class="toggle__3ZBJ"></div></div></span></div><div><p>Given two sorted integer arrays <em>nums1</em> and <em>nums2</em>, merge <em>nums2</em> into <em>nums1</em> as one sorted array.</p>

<p><strong>Note:</strong></p>

<ul>
	<li>The number of elements initialized in <em>nums1</em> and <em>nums2</em> are <em>m</em> and <em>n</em> respectively.</li>
	<li>You may assume that <em>nums1</em> has enough space (size that is greater or equal to <em>m</em> + <em>n</em>) to hold additional elements from <em>nums2</em>.</li>
</ul>

<p><strong>Example:</strong></p>

<pre><strong>Input:</strong>
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

<strong>Output:</strong>&nbsp;[1,2,2,3,5,6]
</pre>
</div></div>

### 2、 中文版

<div class="question-description__3U1T"><div class="translation-tool__3Ffj"><span class="" data-toggle="tooltip" data-placement="left" data-original-title="显示英文" aria-hidden="true" style="cursor: pointer;"><div class="switch-base__1Zql" data-on="false"><div class="toggle__3ZBJ"></div></div></span></div><div><p>给定两个有序整数数组&nbsp;<em>nums1 </em>和 <em>nums2</em>，将 <em>nums2 </em>合并到&nbsp;<em>nums1&nbsp;</em>中<em>，</em>使得&nbsp;<em>num1 </em>成为一个有序数组。</p>

<p><strong>说明:</strong></p>

<ul>
	<li>初始化&nbsp;<em>nums1</em> 和 <em>nums2</em> 的元素数量分别为&nbsp;<em>m</em> 和 <em>n</em>。</li>
	<li>你可以假设&nbsp;<em>nums1&nbsp;</em>有足够的空间（空间大小大于或等于&nbsp;<em>m + n</em>）来保存 <em>nums2</em> 中的元素。</li>
</ul>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong>
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

<strong>输出:</strong>&nbsp;[1,2,2,3,5,6]</pre>
</div></div>

二、ruby方案
----------

```ruby
# 程序是好的，但是leetcode出了莫名其妙的问题，irb测试同一测试用例通过；
# 而且ruby不用考虑m/n越界
# @param {Integer[]} nums1
# @param {Integer} m
# @param {Integer[]} nums2
# @param {Integer} n
# @return {Void} Do not return anything, modify nums1 in-place instead.
def merge(nums1, m, nums2, n)
  array1 = nums1[0...m]
  array2 = nums2[0...n]
  (array1 + array2).sort!
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

