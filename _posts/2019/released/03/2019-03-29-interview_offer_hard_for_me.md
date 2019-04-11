---
layout:     post
title:      剑指offer题目思路总结
category: better
tags: [better]
excerpt: 一个人有多努力，就会有多幸运。
---

剑指offer题目思路总结
=======================================

剑指offer中对我来说比较有挑战的，代码实现一下。

一共32道编码题目。

```html
当做题没有思路的时候，超三个方向想，

1、画图
2、举例
3、分解

当看到一个题目时，首先应该想到的两点

什么算法思想
什么数据结构定义

然后递归就考虑栈，最优化就想动归，棋盘问题就是回溯；
```

------------------------------------------------


第六章
----------------------

#### 面试题55： 二叉树的深度
##### 题目1： 二叉树的深度

deepth(node.left) > deepth(node.right) ? deepth(node.left) + 1 : deepth(node.right) + 1

##### 题目2： 平衡二叉树

思路： 左右子树相差不超过1

if |deepth(node.left) - deepth(node.right)| return false

else is_blance(node.left) && is_blance(node.right)

##### 代码
优化： 记录每个节点的深度

#### 面试题59： 队列的最大值(代码)

##### 题目1： 队列的最大值

算法：维护一个最大值队列

和维护的一个最大值栈类似

```ruby
```

#####：题目2： 队列的最大值（代码）

#### 面试题60： n个骰子的点数（代码）

思路：

第一个筛子[1~6], 其他筛子s - 第一个；  递归思想

存储和为s的数组

```
def fun(n, s)

  res = []
  _fun(n, s, res, [])

  res.count / 6 ** n
end

def _fun(n, s, res, temp)
  if res.count == n
    res << temp
  end

  1.upto(6).each do |i|
    temp << i
    _fun(n - 1, s - i, res)
  end
end

```

#### 面试题63： 股票的最大利润

这个题很简单，diff(i) 卖出价是第i天时的价格利润最大，找i-1天前的最小买入值；

##### 扩展： 不止一天 (代码)

#### 面试题64： 求1 + 2 + 3 + ... + n (代码)

#### 面试题65： 不用加减乘除做加法（代码）

第五章
----------------------

#### 面试题40： 最小的k个数

思路一： 排序 (按照上述题目的思路，快排，判断下标)

思路二： 最大堆或红黑树 (以后可以代码)

#### 面试题41： 数据流中的中位数（代码）

维护一个最大堆和最小堆，二者个数相差不超过1（偶数个插入最小堆，奇数个插入最大堆）

如果插入最小堆的数比最大堆的max小，就交换之后插入。

维护两个堆。

#### 面试题42: 连续子数组的最大和

我一直担心的情况是： sum 加了一堆负数之后没有原来大

其实这种担心是多余的，max_sum只记录当前最大值。

```ruby
def find_max_sum_of_sub_array(array)
  if array.nil? || array.empty?
    return 0
  end

  sum = 0
  max_sum = -2 ** 31
  0.upto(array.size - 1).each do |index|
    if sum <= 0
      sum = array[index]
    else
      sum += array[index]
      if sum > max_sum
        max_sum = sum
      end
    end
  end

  max_sum
end
```

动态规划思想， f(i) 表示以i结尾的数组的最大和

f(i) = max{ f(i-1) + d[i], d[i]}

```ruby
def find_max_sum_of_sub_array(array)
  if array.nil? || array.empty?
    return 0
  end

  res = [array[0]]
  1.upto(array.size - 1).each do |index|
    res[index] = array[index] > (res[index-1] + array[index]) ? array[index] : (res[index-1] + array[index])
  end

  res.max
end
```

#### 面试题43： 1~n整数中1出现的次数

暴力枚举

算法没懂，排列组合的计算， 以后可以代码。

#### 面试题45： 把数组排成最小的数

思路一： 全排列排序

```ruby
def _fun(array)
  return nil if array.nil?

  res = []
  fun(array, 0, res)

  res.min
end

def fun(array, index, res)
  if index == array.size - 1
    res << array.join.to_i
    return
  end

  temp = array[index]
  index.upto(array.size - 1).each do |i|
    array[index] = array[i]
    array[i] = temp

    fun(array, index + 1, res)

    array[i] = array[index]
    array[index] = temp
  end
end

```


思路二： 我记得之前做过一个最大值
[largest_number](https://github.com/hunzino1/leetcode_ruby/blob/master/algorithms/largest_number.rb)

<=> 是比较运算符， 左边大于右边返回正数，左边小于右边返回负数，相等返回0

```
##
# 1、转字符串
# 2、排序 x1 + x2 > x2 + x1
# 3、链接
# 4、去掉前面多余的0
def nums
  n = nums.map(&:to_s).sort{|x1, x2| x1 + x2 <=> x2 + x1}.reduce(&:+)
  n[(n.index(/[^0]/) || -1)..-1]
end
```

#### 面试题46： 把数字翻译成字符串

思路： 递归

就像上面的组合，分两种情况递归，以12258为例

1、 1 + 2258

2、 12 + 258

```ruby
def num_2_string(num)
  return "" if num.nil?

  num_str = num.to_s

  str = ('a'..'z').to_a
  map_num = {}
  ('1'..'26').to_a.each do |num|
    map_num[num] = str[num.to_i - 1]
  end

  map_string(num_str, map_num, "", 0)
end

def map_string(num_str, map_num, cur_str, index)
  if index == num_str.size - 1
    p cur_str
    return
  end

  map_string(num_str, map_num, cur_str + num_str[index], index + 1)

  if index < num_str.size - 1 && !num_str[index:index + 1].nil? && num_str[index:index + 1] <= '26'
    map_string(num_str, map_num, cur_str + num_str[index:index + 1], index + 2)
  end
end
```

递归存在大量的重复，自右向左分析

也是分成1个 或者 2个的情况

#### 面试题47： 礼物的最大价值

思路：

因为只有向右或者向下，不必使用回溯

递归： f(i,j) = max(f(i - 1, j), f(i, j - 1)) + d[i][j]

避免重复操作，可以使用非递归的操作


```ruby
# 坐标的合法性（越界）
def max_value(matrix, rows, cols)
  return 0 if matrix.nil? || matrix.empty?

  res = Array.new(rows) {Array.new(cols, 0)}
  res[0][0] = matrix[0][0]

  0.upto(rows - 1).each do |row|
    0.upto(cols - 1).each do |col|
      res[row][col] = matrix[row][col] + (res[row - 1][col] > res[row][col - 1] ? res[row - 1][col] : res[row][col - 1])
    end
  end

  res[rows - 1][cols - 1]
end
```

#### 面试题48： 最长不含重复字符的子串

思路：最长，最优化算法 - 动归

动态规划：f(i) 以第i个字符结尾的最长子串

d[i]:  计算和d[i]重复的字符，二者之间的距离d

  d 大于f(i-1), 说明不在f(i-1)的子串中
  d 小于f(i-1)

```ruby
def longest_sub_string(str)
  return nil if str.nil? || str.empty?

  res = [1]

  1.upto(str.size - 1).each do |index|
    ch = str[index]
    if str[0...index].include?(ch)
      rindex = str[0...index].rindex(ch)
      distance = index = rindex
      if res[index - 1] < distance
        res[index] = res[index - 1] + 1
      else
        res[index] = distance
      end
    else
      res[index] = res[index - 1] + 1
    end
  end

  res.max
end
```


第四章
------------------

#### 面试题27： 二叉树镜像

思路： 以此交换节点的左右节点

```ruby
def mirror_tree(node)
  return if node.nil?
  return node if node.left.nil? && node.right.nil?

  p_temp = node.left
  node.left = node.right
  node.right = p_temp

  if node.left
    mirror_tree(node.left)
  end

  if node.right
    mirror_tree(node.right)
  end
end
```

#### 面试题28： 对称的二叉树

前序遍历 和 右子树的前序遍历一致

然后nil也写入，递归的遍历方案很好

```ruby
def is_mirror_tree?(proot)
  return true if proot.nil?

  return is_mirror_tree?(proot.left, proot.right)
end

def is_mirror_tree?(proot1, proot2)
  return true if proot1.nil? && proot2.nil?
  return false if proot1.nil? || proot2.nil? || proot1.val != proot2.val

  return is_mirror_tree?(proot1.left, proot2.right) && is_mirror_tree?(proot1.right, proot2.left)
```

#### 面试题33： 二叉搜索树的后序遍历序列

思路：用根节点把数组分为两个数组，判断数组合法性

```ruby
def is_bts_squence(array)
  return true if array.nil? || array.size <= 2
  
  root_val = array[-1]
  node = array.find {|e| e > root_val}
  return is_bts_squence(array[:-2]) if node.nil?

  index = array.index(node)
  node = array[index:-1].find {|e| e < root_val}
  if node.nil?
    return is_bts_squence(array[0...index]) && is_bts_squence(array[index:-2])
  else
    return false
  end
end
```

#### 面试题34： 二叉树中和为某一值的路径

果断深搜

```ruby
def is_exist_path?(root, val)
  return false if root.nil? && val != 0
  return true if root.nil? && val == 0

  retrun is_exist_path?(root.left, val - root.val) || is_exist_path?(root.right, val - root.val)
end
```

### 面试题35： 复杂链表的复制

思路1：

1. 复制每一个节点，并用next指针连接

2. 在复制的同时创建一个{node, node'}的hash表；

思路2：

1. 创建 A A' B B' ... E E', 就是原来的链表x2

2. 然后呢，如果A -> B,  那么A' -> B'（B的后置节点）；

3. 将偶数位穿起来，即为所求。

### 面试题36： 二叉搜索树和双向链表

思路： 分成三部分，根节点，左子树，右子树

因为有序所以中序遍历，当前节点node， node的左、右节点增加一个指向node的指针，（对应的分别是前一个节点，后一个节点）

递归思路解决。

中序遍历是有序的。

递归思路不太懂，整个非递归思路的：

```ruby
# 主要思路是遍历每个节点都是先将该节点自身以及最左节点依次入栈
# 然后每次出栈一个节点之后重复上述步骤
#
def convert(root)
  return nil if root.nil?
  # p 是遍历指针
  # pre 指向上一次遍历的节点
  p, pre = root, nil

  stack = []
  # 只有第一次有用，找双向链表的头结点，用root标识
  is_head = true

  while !p.nil? || !stack.empty? do
    # 当前节点，以及最左节点依次进栈
    while !p.nil? do
      stack.push p
      p = p.left
    end

    # 取最后一个左节点
    p = stack.pop
    if is_head
      root = p
      pre = p
      is_head = false
    else
      pre.last = p
      p.pre = pre
      pre = p
    end

    p = p.right
  end

  root
end
```

### 面试题37： 序列化二叉树

思路： 将nil也放入list， 先序即可

```ruby
###
# 序列化
#
serialize_list = []

def serialize(root, serialize_list)
  if root.nil
    serialize_list << '$,'
    return
  end

  serialize_list << root.val << ','
  serialize(root.left, serialize_list)
  serialize(root.right, serialize_list)
end

def deserialize(root, serialize_list)
  root = new TreeNode
  root.val = serialize_list.shift
  root.left = nil
  root.right = nil

  deserialize(root.left, serialize_list)
  deserialize(root.right, serialize_list)
end

```

### 面试题38： 字符串的排列

思路：

如下，分两个步骤

1、先确定第一个字符； 将第一个字符一次和后面的交换，确定第一个字符之后固定

2、全排列后面的字符

题目中我用index来标识，index之前的字符已经确定，需要递归全排列index:-1的字符。

```ruby
def _permutation(str)
  if str.nil?
    retrun
  end

  index = 0
  permutation(str, index)
end

def permutation(str, index)
  size = str.size
  if index == size -1
    p str
  end

  ch = str[index]
  index.upto(size - 1).each do |i|
    # 交换
    str[index] = str[i]
    str[i] = ch

    permutation(str, index + 1)

    # 递归之后要交换回来
    str[i] = str[index]
    str[index] = ch
  end
end

```

拓展：

1、字符串的组合

两种情况：

n个字符打印m个字符的情况：

1、选中当前字符， 在剩余字符中选取m-1个字符

2、未选中当前字符，在剩余字符中选取m个字符

```ruby
def _permutation(str)
  return if str.nil?

  1.upto(str.size).each do |count|
    # 从str选出count的组合
    permutation(str, "", count, 0)
  end
end

##
# str: 字符串
# cur_str： 字符串组合选项
# count： 字符串组合长度
# index: 遍历str的下标
#
def permutation(str, cur_str, count, index)
  if cur_str.size == count
    p cur_str
    return
  elsif index == str.size
    return
  end

  ch = cur_str + str[index]
  permutation(str, ch, count, index + 1)
  permutation(str, cur_str, count, index + 1)
end
```

2、正方体

思路： 获取a1-a8的全排列，筛选出满足条件的组合

```ruby
def _permutation(str)
  return if str.nil?

  permutation(str,  0)
end

def permutation(str, index)
  if index == str.size - 1
    p str
  end

  ch = str[index]
  index.upto(str.size - 1).each do |i|
    str[index] = str[i]
    str[i] = ch

    permutation(str, index + 1)

    str[i] = str[index]
    str[index] = ch
  end
end
```

3、8皇后

思路：

1、初始化8行； 因为不同行

2、0~7全排列，然后此时不同行、不同列

3、筛选出对角线符合的选项。

第三章
-------------------

### 面试题17： 打印从1到最大n位数

数字排列问题

主要是递归的终止条件怎么判断

```ruby
def print_n_num(n)
  if n <= 0
   print 0
   return
  end

  0.upto(9).each do |index|
    str = index.to_s
    print_num_of_n(str, 0, n)
  end
end

def print_num_of_n(str, index, n)
  if n == index + 1
    p str
  end

  0.upto(9).each do |index|
    str += index.to_s
    print_num_of_n(str, index + 1, n)
  end
end
```

### 面试题19： 正则表达式匹配

非确定有限状态机，逐个字符比对，逐个情况分析

```ruby
# 主要就是第二个字符是"*"的情况分析清楚
def match(str, pattern)
  return false if str.nil? || pattern.nil?
  return true if str.empty? && pattern.empty?
  return false if !str.empty? && pattern.empty?
  return true if str == pattern

  if pattern[1] == '*'
    if str && (str[0] == pattern[0] || pattern[0] == '.')
      return match(str, pattern[2 : -1]) || 
        match(str[1 : -1], pattern) ||
        match(str[1 : -1], pattern[2 : -1])
    else
      return match(str, pattern[2 : -1])
  elsif str[0] == pattern[0] || pattern[0] == '.'
    return match(str[1 : -1], pattern[1 : -1])
  end
  false
end
```

### 面试题20 表示数值的字符串

A.B(e|E)C 或者 .B(e|E)C

思路：如上
A: 整数部分
B: 小数部分
C：指数部分

思路：按A/B/C三部分扫描，遇到数字，尽可能多扫描数字；

然后遇到'.'，保存为B

运到e/E， 保存为C

```ruby
##
# A和C可能带正负号，B是无符号整数
#
def is_number?(str)
  return false if str.nil?
  ['.', 'e', 'E'].each do |char|
   return false if is_illgal?(str, char)
  end

  index = scan_integer(str)
  if index == -1
    return true
  elsif index == 0
    if str[0] != '.'
      return false
    else
      index = scan_nosign_integer(str[1:-1])
    end
  else
    if str[index + 1] == '.'
      index = scan_nosign_integer(str[index + 2:-1])
    end
  end

  if index == -1
    return true
  elsif str[index + 1] == 'e' || str[index + 1] == 'E'
    if scan_integer(str[index + 2:-1]) == -1
      return true
    else
      return false
    end
  else
    false
  end

  false
end

def is_illgal?(str, ch)
  return str.count(ch) > 1
end

def scan_integer(str)
  if str[0] == '+' || str[0] == '-'
    return scan_nosign_integer(str[1:-1])
  else
    return scan_nosign_integer(str)
  end
end

def scan_nosign_integer(str)
  str.each_char do |char|
    if char < '0' || char > '9'
      return str.index(char)
    end
  end

  -1
end
```


#### 面试题25： 合并两个有序链表

递归的思路很棒

```ruby
def merge(phead_1, phead_2)
  return phead_1 if phead_2.nil?
  return phead_2 if phead_1.nil?

  new_head = nil
  if phead_1.val < phead_2.val
    new_head = phead_1
    new_head.next = merge(phead_1.next, phead_2)
  else
    new_head = phead_2
    new_head.next = merge(phead_1, phead_2.next)
  end

  new_head
end
```

#### 面试题26: 树的子结构

思路：

1、找到父树中存在子树的根节点；

就是树的遍历

2、判断该根节点的树是不是包括子树结构；

```ruby
def has_sub_tree(proot_1, proot_2)
  res = false

  if !proot_1.nil? && !proot_2.val
    if proot_1.val == proot_2.val
      res = does_tree1_have_tree2(proot_1, proot_2)
    end
    if !res
      res = has_sub_tree(proot_1.left, proot_2)
    end
    if !res
      res = has_sub_tree(proot_1.right, proot_2)
    end
  end

  res
end

def does_tree1_have_tree2(proot_1, proot_2)
  return true if proot_2.nil?
  return false if proot_1.nil?

  return false if proot_1.val != proot_2.val

  return does_tree1_have_tree2(proot_1.left, proot_2.left) &&
    does_tree1_have_tree2(proot_1.right, proot_2.right)
end
```

第二章
-----------------------

### 面试题6： 从尾到头打印链表

如果不会递归方式，可以把链表先一个个入栈，然后再出栈打印。

递归

```ruby
def print_reverse(node)
  if !node.nil?
    if !node.next.nil?
      print_reverse(node.next)
    end

    print node.val + " "
  end
end
```

### 面试题8： 二叉树的下一个节点

有右子树

          父节点的左节点
无右子树
          父节点的右节点
```ruby
class TreeNode
  attr_incessor: :val, :pre, :left, :right
  def initialize(val)
    @val   = val
    @pre   = nil
    @left  = nil
    @right = nil
  end

  def find_next_node(node)
    return if node.nil?

    #右子树的最左节点
    if (right = node.right)
      return right.val if reght.left.nil?
      left = reght.left
      while left do
        left = left.left
      end
      return left.val
    # 父节点分两种情况
    else
      pre = node.pre
      return if pre.nil?
      # 父节点的左节点
      if pre.left == node
        return pre.val
      else
        while pre do
          tmp = pre.pre
          if tmp.left = pre
            return tmp.val
          end
        end
      end
    end
    return
  end
end
```

```html
面试题12，13都是用的回溯，回溯的基本思想就是 穷举 + 深搜

见到二维数组就能考虑这种方法。
```

### 面试题12： 矩阵中的路径

```ruby
def have_path?(matrix, string, rows, cols)
  if string.nil? || matrix.nil? || matrix.empty?
    return false
  end

  return true if string.empty?

  path_length = 0
  has_visited = Array.new(arrays.size) {Array.new(arrays.first.size, false)}

  col, row = 0, 0
  while row < rows do
    while col < cols do
      if is_exist_path?(matrix, row, col, rows, cols, string, has_visited, path_length)
        return true
      end
    end
  end
end

def is_exist_path?(matrix, row, col, rows, cols, string, has_visited, path_length)
  return true if string.length == path_length

  has_path = false
  if (row < rows && row >= 0 && col < cols && col >= 0 &&
      matrix[row][col] == string[path_length] && !has_visited[row][col])
      path_length += 1
      has_visited[row][col] = true
  end

  has_path = is_exist_path?(matrix, row, col - 1, rows, cols, string, has_visited, path_length) ||
        is_exist_path?(matrix, row - 1, col, rows, cols, string, has_visited, path_length) ||
        is_exist_path?(matrix, row + 1, col, rows, cols, string, has_visited, path_length) ||
        is_exist_path?(matrix, row, col + 1, rows, cols, string, has_visited, path_length)

  if !has_path
    path_length -= 1
    has_visited[row][col] = false
  end

  has_path
end
```

### 面试题13： 机器人的运动范围

感觉题目有问题，只要check(row, col) < k, 直接累加即可。

但是还是按照树上的代码练一下回溯。

```ruby
def max_path_robot_reach(matrix, rows, cols, k)
  return 0 if rows <=0 || cols <= 0 || k <= 0

  has_visited = Array.new(rows) {Array.new(cols, false)}
  path = 0

  path_count = can_reach_count(matrix, 0, 0, rows, cols, has_visited, k)
end

def can_reach_count(matrix, row, col, rows, cols, has_visited, k)
  count = 0
  if (row >= 0 && col >= 0 && row < rows && col < cols &&
      !has_visited[row][col] && (get_sum(row) + get_sum(col)) < k)
      has_visited[row][col] = true
      count = 1 + can_reach_count(matrix, row - 1, col, rows, cols, has_visited, path, k) +
            can_reach_count(matrix, row, col - 1, rows, cols, has_visited, path, k) +
            can_reach_count(matrix, row + 1, col, rows, cols, has_visited, path, k) +
            can_reach_count(matrix, row, col + 1, rows, cols, has_visited, path, k)

  end

  count
end

def get_sum(num)
  sum = 0
  while num > 0 do
    sum += num % 10
    num = num / 10
  end
  sum
end
```

```html
动态规划，首先最优化问题都是先想到动态规划；

而动态规划的思路就是状态方程，然后用数组的形式实现。
```
### 面试题14： 剪绳子

递推状态方程：

f(n) 表示长度为n的绳子剪成若干段后的最大乘积

之前不理解题意，切几段是不定的，所以参数中并没有段数限制

所以 f(n) = max( f(i) * f(n-i))  0 <= i <= n

```ruby
##
# length: 绳子长度
# m:      裁剪段数
#
#def max_multiply_of_choice(length, m)
#  if length < 2
#    return length
#  end
#
#  # 至少一刀
#  if length == 3 || length == 2
#    return (length - 1)
#  end
#
#  cut_max_res = [0, 1, 2, 3]
#  max = 0
#end

##
# length: 绳子长度
# 至少一刀
#
def max_multiply_of_choice(length)
  return 0 if length <= 1
  return length - 1 if length <= 3

  # 绳子长度为0，1，2...length时能够获得的最大裁剪乘积
  max_res_of_every_length = [0, 0, 1, 2]

  # 遍历从长度为4到length的绳长
  4.upto(length).each do |every_length|
    max = every_length
    # i的取值最大值为当前绳长的一半即可
    1.upto(every_length / 2).each do |cut_point|
      res = max_res_of_every_length[cut_point] * max_res_of_every_length[every_length - cut_point]
      if res > max
        max = res
      end
    end
    max_res_of_every_length[every_length] = max
  end

  max_res_of_every_length[length]
end
```

### 面试题15： 二进制中1的个数

思路： n & (n - 1)总是会将n的二进制中右边第一个1给消除。

```ruby
def num_of_1(n)
  num = 0
  while num > 0 do
    num += 1
    num = num & (num - 1)
  end
  num
end
```


