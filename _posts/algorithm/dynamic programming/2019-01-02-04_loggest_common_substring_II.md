---
layout:     post
title:      动态规划 - 最长公共子序列(连续)
category: better
tags: [better]
excerpt: 坚持这件小事，20190424打卡。
---


动态规划 - 最长公共子序列(连续)
=====================================

一、题目描述
----------

![longest_common_sub_ii](https://hunzino1.github.io/assets/images/2019/algorithm/dynamic_programming/longest_common_sub/longest_common_sub_ii.png)

二、ruby方案
----------

### 2.1 思路设计

**不能使用暴力搜索方法。一个长度为n的序列拥有 2的n次方个子序列，它的时间复杂度是指数阶，太恐怖了。**
