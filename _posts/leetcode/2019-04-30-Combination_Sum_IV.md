---
layout:     post
title:      leetcode(377) - 组合总和 Ⅳ
category: better
tags: [better]
excerpt: 坚持这件小事，20190424打卡。
---


[组合总和 Ⅳ](https://leetcode-cn.com/problems/combination-sum-iv/)
=======

一、题目描述
----------

### 1、English版本

<div class="content__2ebE"><div><p>Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.</p>

<p><b>Example:</b></p>

<pre><i><b>nums</b></i> = [1, 2, 3]
<i><b>target</b></i> = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is <i><b>7</b></i>.
</pre>

<p>&nbsp;</p>

<p><b>Follow up:</b><br>
What if negative numbers are allowed in the given array?<br>
How does it change the problem?<br>
What limitation we need to add to the question to allow negative numbers?</p>

<p><b>Credits:</b><br>
Special thanks to <a href="https://leetcode.com/pbrother/">@pbrother</a> for adding this problem and creating all test cases.</p>
</div></div>

### 2、 中文版

<div class="content__2ebE"><div><p>给定一个由正整数组成且不存在重复数字的数组，找出和为给定目标正整数的组合的个数。</p>

<p><strong>示例:</strong></p>

<pre><em><strong>nums</strong></em> = [1, 2, 3]
<em><strong>target</strong></em> = 4

所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

请注意，顺序不同的序列被视作不同的组合。

因此输出为 <strong>7</strong>。
</pre>

<p><strong>进阶：</strong><br>
如果给定的数组中含有负数会怎么样？<br>
问题会产生什么变化？<br>
我们需要在题目中添加什么限制来允许负数的出现？</p>

<p><strong>致谢：</strong><br>
特别感谢&nbsp;<a href="https://leetcode.com/pbrother/">@pbrother</a>&nbsp;添加此问题并创建所有测试用例。</p>
</div></div>

二、ruby方案
----------

### 思路1： 动归

```html
dp[i]代表target为i时使用nums中的数能组成的组合数的个数

递推公式：

dp[i] = dp[i-nums[0]] + dp[i-nums[1]] + dp[i=nums[2]]+ ...

举个例子比如nums=[1,3,4],target=7;

dp[7]=dp[6]+dp[4]+dp[3]

其实就是说7的组合数可以由三部分组成，1和dp[6]，3和dp[4],4和dp[3];
因为不同顺序的组合也算一种，所以可以使用这种nums顺序排列。
```

代码如下：

```ruby
# @param {Integer[]} nums
# @param {Integer} target
# @return {Integer}
def combination_sum4(nums, target)
  return 0 if nums.nil? || nums.empty?

  # 因为都是正数，所以长度最多是target,target个1组成。
  dp = Array.new(target + 1, 0)
  # 其实target为0时，只有0种可能，但是为了计算dp[1]中除去dp[0]因素，所以赋值
  dp[0] = 1

  i = 1
  while i <= target do
    nums.each do |num|
      if i >= num
        dp[i] += dp[i - num]
      end
    end
    i += 1
  end

  dp[target]
end
```

### 思路二 记忆化搜索

略
