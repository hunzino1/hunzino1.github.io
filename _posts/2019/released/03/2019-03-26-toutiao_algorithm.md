---
layout:     post
title:      头条面试 - 找出[1,n]范围内按照字典排序的最小第k个值
category: interview
tags: [interview]
excerpt: 一个人有多努力，就会有多幸运。
---

找出[1,n]范围内按照字典排序的最小第k个值
=======================================

1 题目描述
---------------------------

 给定一个数组序列, 需要求选出一个区间, 使得该区间是所有区间中经过如下计算的值最大的一个：

区间中的最小数 * 区间所有数的和最后程序输出经过计算后的最大值即可，不需要输出具体的区间。如给定序列  [6 2 1]则根据上述公式, 可得到所有可以选定各个区间的计算值:


2 代码
---------------------------

最后一种代码好理解

```java
import java.util.Scanner;
public class Main {
public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        int size = sc.nextInt();
        int [] input  =new int [size];
        for(int i=0;i<input.length;i++){
            input[i] = sc.nextInt();
        }

        long max=input[0]*input[0];
        for(int i=0;i<size;i++){
             long sum=input[i];
             int min=input[i];
            for(int j=i+1;j<size;j++){
                if(input[j]==0){
                    break;
                }
                sum+=input[j];
                if(input[j]<min){
                    min = input[j];
                }
                if(min*sum>max){
                    max=min*sum;
                }
            }
        }
        System.out.println(max);
    }
}
```

```ruby
单调栈

def find_max(nums)
  length = nums.size
  # 预处理，先将累计和保存；若在出栈时再计算区间和，超时AC 80%
  pre_sum = [nums[0]]
  1.upto(nums.size - 1).each do |i|
    pre_sum[i] = pre_sum[i-1] + nums[i]
  end

  # 先将第一个数字下标压入栈
  index_satck = [0]
  max_multi = 0
  cur_index = 1
  # 数组遍历结束但是栈不为空时仍要继续
  while cur_index < length || (cur_index == n && index_satck.size > 0) do
    # 当栈为空或者当前数大于等于栈顶元素，下标入栈
    if cur_index < length && (index_satck.empty? || nums[cur_index] >= nums[index_stack[-1]])
      index_satck << cur_index
      cur_index += 1
    # 当数组遍历结束或者当前数小于栈顶元素时
    else
      # 区间最小值为栈顶元素，出栈
      min_num = nums[index_stack.pop]
      # 如果此时栈为空了，表示已出栈的数字比前面所有数字都小，左边界下标也就是0，否则左边界下标为（下一个元素的索引+1）
      cur_sum = pre_sum[cur_index - 1] - (index_stack.empty? ? pre_sum[index_stack[-1]] : 0)
      cur_max = min_num * cur_sum
      max_multi = max_multi > cur_max ? max_multi : cur_max
    end
  end

  max_multi
end
```

```ruby
size = gets.to_i
input = []
0.upto(size - 1).each do |index|
  input << gets.to_i
end

max = input[0] ** 2

0.upto(size - 1).each do |index|
  sum = input[index]
  min = input[index]

  (index + 1).upto(size - 1).each do |next_index|
    break if input[next_index] == 0

    sum += input[next_index]

    if input[next_index] < min
      min = input[next_index]
    end

    if min * sum > max
      max = min * sum
    end
  end
end

p max
```
1、P为给定的二维平面整数点集。定义 P 中某点x，如果x满足 P 中任意点都不在 x 的右上方区域内（横纵坐标都大于x），则称其为“最大的”。求出所有“最大的”点的集合。（所有点的横坐标和纵坐标都不重复, 坐标轴范围在[0, 1e9) 内）

如下图：实心点为满足条件的点的集合。请实现代码找到集合 P 中的所有 ”最大“ 点的集合并输出。

2.变治法（预排序）

由“最大点”的性质可知，对于每一个“最大点”，若存在其他点的y值大于该点y值，那么其他点x值必然小于该点的x值。

换言之，当某一点确定它的x值高于所有y值大于它的点的x值，那么该点就是“最大点” 。网上给出的答案基本上都是这个套路。

对于y有序的点集，只需要O(n)即可输出“最大点”点集。一般基于比较的排序算法时间复杂度O(nlogn)。那么，显而易见，算法整体复杂度为O(nlogn)。

3.减治法+变治法（过滤+预排序）

先预处理一下

过滤很简单，就是在集合中找出一个比较好的点，然后过滤掉其左下角的所有点。然后再采用方法2对过滤后的点集求解。

那么这个集合中比较好的点，怎么找，或者说哪个点是比较好的点。显而易见，越靠近点集右上角的点，左下角的面积就越大，越可以过滤更多的点，故越好。

儿时学过，两个数的和一定，那么两数差越小，乘积越大。简单设计，该点x和y的和减去x和y差的绝对值越大，该点越好。



```ruby
count = gets
# 1.字符串要转数字,否则index < count 会报数据类型错误
count = count.to_i
point_list = []
index = 0

while index < count do
  x = gets
  y = gets
  point_list << {x: x.to_i, y: y.to_i}
  index += 1
end

# 2、按照x坐标排序， 用的还是sort_by而不是sort
sorted_point_list = point_list.sort_by{|point| point[:x]}

# 不太可行，因为删除一个数据之后index，cur_index会永远小于count,所以count也要做减法
# 逐个比较y坐标
index = 0
while index < count do
  # 因为在一直删除，所以要加判断
  break if sorted_point_list[index].nil?
  cur_point_y = sorted_point_list[index][:y]

  point = sorted_point_list.find {|point| point[:y] > cur_point_y}
  if point.nil?
    index += 1
  else
    sorted_point_list.delete_at(index)
    count -= 1
  end

  <!-- next_index = index + 1 -->
  <!-- while next_index < count do -->
  <!--   # 4、也要加非空判断 -->
  <!--   break if sorted_point_list[next_index].nil? -->
  <!--   next_point_y = sorted_point_list[next_index][:y] -->
  <!--   if next_point_y > cur_point_y -->
  <!--     break -->
  <!--   end -->
  <!--   next_index += 1 -->
  <!-- end -->
  <!--  -->
  <!-- # 3、说明存在x,y均大于改点的坐标，这时下标不应该增加 -->
  <!-- if next_index < count -->
  <!--   sorted_point_list.delete_at(index) -->
  <!--   count -= 1 -->
  <!-- else -->
  <!--   index += 1 -->
  <!-- end -->
end

# 输出sorted_point_list
p "" if sorted_point_list.empty?
sorted_point_list.each do |point|
  print point[:x] + " " + point[:y]
  p ""
end

```
