---
layout:     post
title:      leet801 - 使得序列递增的最小交换
category: better
tags: [better]
excerpt: 坚持这件小事，20190424打卡。
---

[leetcode801 - 使得序列递增的最小交换](https://leetcode-cn.com/problems/minimum-swaps-to-make-sequences-increasing/)
=================================

一、题目描述
----------


二、ruby方案
----------

['1','2','3','1'].sort{|x, y| x + y <=> y + x}

nums.map(&:to_s).sort{|x1, x2| x1 + x2 <=> x2 + x1}
