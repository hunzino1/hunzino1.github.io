---
layout:     post
title:      每日算法 - 捕捉雨水（Trapping Rain Water）
category: better
tags: [better]
excerpt: 一个人有多努力，就会有多幸运。
---

捕捉雨水（Trapping Rain Water）
=======================================

1.题目描述
----------------------

```html
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped. Thanks Marcos for contributing this image!

Example:

Input: [0,1,0,2,1,0,1,3,2,1,2,1] 

Output: 6
```

![捕捉雨水](https://hunzino1.github.io/assets/images/2019/day_task/rain.png)

2.题目分析
--------------------

```html
这道题目看似比较复杂，但是仔细观察题目所给出的图片信息你就会发现一个规律，加入水后，图片的高度整体是从左右两边往中间的一个最高点单向增大的，所以这就给了我们一个很好的解题思路。

先找到整张图的最高点，并记录它所在的位置;
然后依次从左右两端往该点遍历，遍历时记录左边遍历过的位置的最高点的值，当到达位置的实际高度小于我们记录的最高值，则表明此处有积水，
由于宽度都是1，所以我们总积水的体积可以直接加上在此处实际高度与记录的最高高度的差值。

当左右遍历完成后，所得结果及时总的积水体积。

找数组中最大的值，再从这个数左右方向分别找对应的次最大值max，再求区间的各个差值之和。 代码如下

```

3.代码
-----------------

```java
public class Solution {
    public int trap(int[] height) {
        // for(int i=0;i<height.length()-2;i++){

        // }

        int secHight = 0;
		int left = 0;
		int right = height.length-1;
		int area = 0;
		while (left < right){
			if (height[left] <height[right]){
				secHight = Math.max(height[left], secHight);
				area += secHight-height[left];//计算当前格的能装雨水的容量
				left++;
			} else {
				secHight = Math.max(height[right], secHight);
				area += secHight-height[right];
				right--;
			}
		}
		return area;
    }
}
```

```ruby
def trap(height_list)
  size = height_list.size
  return 0 if size <= 2

  # 最高位
  max_height = height_list.max
  max_index = height_list.index(max_height)

  # 次高位, 从左边开始
  sec_left = height_list[0]
  sec_right = height_list[-1]

  # 从左右向中间推
  left, right = 0, size - 1

  # 存水量
  area = 0
  # 左右遍历
  while (left < max_index) || (right > max_index) do
    if left < max_index
      if sec_left < height_list[left]
        sec_left = height_list[left]
      else
        area += (sec_left - height_list[left])
      end
      left += 1
    end

    if right > max_index
      if sec_right < height_list[right]
        sec_right = height_list[right]
      else
        area += (sec_right - height_list[right])
      end
      right -= 1
    end
  end
  area
end
```
