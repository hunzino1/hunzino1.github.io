---
layout:     post
title:      经典算法思想总结
no-post-nav: true
category: algorithmtree
tags: [algorithmtree]
excerpt: 今天比较烦
---

> 有意想学一下算法，但是找来找去总是觉得不太满意。
> 查看一些算法的博客，总感觉有一些问题"无意间"绕过去了，干脆自己总结一个。
> 常见的算法思想: 穷举、分治、贪心、dp、回溯、分支限界。
> 我觉得学习算法无非弄懂三点：
- 算法思想的定义，解决什么问题？
- 什么样的场景回去选择这种算法思想？(贪心好多人都避而不提，造成很多疑惑)
- 确定了某种算法思想之后，如何用代码实现，即思考的方向是什么？

> 其中前两点是最重要的，而代码如何实现掌握的最好途径就是练习。
> 所以主要总结前两点；

# 一 穷举思想(枚举思想)
### 1、枚举定义：
> 枚举,顾名思义，就是对所有可能的解一个一个的列举。
> 但是，同时也是基于已有知识对答案进行猜测的一种策略，也就是说，如果策略选择的恰当，枚举也是一种有效的解决方案。

      举例：
        求小于N的最大素数？
          1、首先求小于N的最大素数是没有公式的；
			    2、那么只能使用枚举，逆序看 N-1、N-2 ….是素数吗？
			    3、N-k是素数的条件： N-K不能被任何大于1小于N-k的素数整除；
		    归结以上三点，整个枚举过程其实就是，求出小于N的所有素数，这个是与问题等价的。
		    求小于N的最大素数  ==  求出小于N的所有素数

	    求解：
		    1、2是素数 PRM0
		    2、PRM1….PRMk， 如果PRMk+1 是素数，但是大于 N
		    3、 PRMk是小于N的最大素数。

### 2、枚举使用的场景
> 枚举使用的场景很好理解，其实就是一种暴力解法，只要是有限个可能性、可以一一列举的都可以使用枚举算法。

- 解空间是有限个，且相互独立

### 3、枚举算法的实现思路
> 首先枚举就是对所有可能的解进行猜测的过程。
> 根据所知道的知识，给出一个猜测的答案，例如2是素数。

- 1、首先列举问题可能解集合的每一项；
- 2、然后根据问题的检验条件去校验解集合中哪些是成立的
- 3、使得问题条件成立的即为程序的输出。

##### 实际代码完成思路

- 1、分析问题，得出解空间，并且降低解空间的数量级。（局部状态：局部状态确定，剩余状态只有一种结果时使用；熄灯问题）
- 2、验证条件。 验证解空间元素是否正确的公式集合。

### 4、使用枚举的三个关键问题
- 1、给出解空间，建立简洁的数学模型（模型中的变量数尽可能少，解之间相互独立）

      例如本例的解空间：
        1、偶数不可能是素数，所以先排除一部分解空间
        2、n不能被[2, n) 之间的任意素数整除，而不是任意整数，这样也能节约计算成本；
- 2、减少搜索的空间

      例如奇数才可能是素数，那么偶数的循环其实绕过更好。
- 3、采用合适的搜索顺序

      搜索空间的遍历顺序要与模型中的条件表达式一致；
      本例就是从2到n的奇数


# 二 递归和分治算法
## 1) 递归

### 1、递归的定义
> 函数直接或者间接调用自身的算法思想。

### 2、什么样的问题适合使用递归？
> 首先看一下递归和枚举的区别。

      共同点：
        都是列举子问题求解。

      不同点：
        枚举：
          子问题之间是平行的，相互独立的。 每个子问题都是问题的一种可能的形式；
        递归：
          子问题和原问题之间是纵向的， 之间会有依赖关系，层层迭代。

> 由对比可以知道，递归适用的问题情形如下：

      1、问题可以拆解为一个个较小规模的子问题求解；
      2、子问题之间相互关联，自底向上由最小规模子问题获取最终解；

### 3、递归算法的实现思路

#### 1、基本设计思想：

       1、 待求问题的解  --->   输入变量x的函数f(x)    也就是说将问题的解用一个函数f(x)去表示

       2、 解决方法：   查找函数g(),   g()需要满足  f(x)  =   g(f(x - 1));    也就是说f(x)的规模在下降的一个 g（）

       3、 由已知的 f(0) 和 g()， 最终求出 f(x)

       推广：
          输入的变量可能是多个 x、y、z.....  x - 1 的变化也可以推广到 x - n,
          总之 递归的过程 是冲着 "出口"的方向。 （问题规模不断缩小的方向）
          我的总结：
          找到f(x) 和 f(0),  g() 是关键

       举例： 大整数乘法

#### 2、代码实现思路：(递归三要点)

- 1、递归式 g(): 如何将问题划分为子问题；
- 2、递归出口
      递归的终止条件
- 3、界函数

      使得问题规模越来越小的函数。

      int factorial（int n） {
        if (n == 0) {
          return 0;
        }

        return n * factorial(n - 1);
      end

      1、递归式  n * f（n - 1）

      2、出口  n == 0

      3、界函数  n - 1

##### 递归缺点：栈溢出

      可以考虑使用全局数组 或者 动态分配数组。

## 2) 分治算法
### 1、分治算法定义
> 分治算法就是将一个求解复杂的问题，平均分成规模相同的子问题，利用递归的思想进而求出问题的解。
> 举例：1、大整数乘法; 2、二分查找；

### 2、什么样的问题可以使用分治算法？

      比如二分查找，直接查找起来是O(n),二分拆解至单一元素，自底向上遍历，最终求解O(logn);

### 3、分治算法代码实现思路？

      divide-and-conquer(P) {
        if(|P| < n0) adhoc(P);   //n0是阈值，问题规模不超过阈值时，用基本子算法解问题。

        divide P into smaller subinstances P1,P2,P3...Pn;   //否则将问题分解成n个相同规模的子问题，分割最好是平均分。

        for(int i = 1; i <= n; i++) {
          yi = divide-and-conquer(Pi);    //子问题递归调用分治算法。
        }

        return merge(y1, y2...yn);    //将分治算法解得的子问题用合并算法合并。
      }

- 大整数乘法
- 棋盘覆盖

# 三 动态规划
### 1、动态规划定义
> 动态规划：也是将原问题拆解为一个个子问题求解，主要解决方法有三种：
- 记忆性递归
- "人人为我"解法
- "我为人人"解法
> 动态规划和递归、分治最大的不同，是适用的问题类型不同。

### 2、什么样的问题可以使用动态规划？
> 动态规划适用的问题需要满足以下两条性质：
- 最优子结构性质：
        首先动态规划本身就是一个求最优解的过程，如背包问题，最值是装入最大价值的物品；
        而动态规划的每个子问题本身也都是最优的，即最优子结构。
        也就是在背包问题中，每一次装入物品的子过程，也都是一个是得背包取得最大价值的最优解。

- 无后效性：
        当前的若干个状态值一旦确定，
        则此后过程的演变就只和这若干个状态的值有关，
        和之前是采取哪种手段或经过哪条路径演变到当前的这若干个状态，没有关系。

        无后效性,也就是所谓的“未来与过去无关”。
        比如，最长上升子序列，当前子问题一定会根据已经算出的局部最优解更新结果，但是至于这个最优解是通过哪个方式得到的，
        已经是过去式了，不影响现在的计算和结果。

> 只有同时满足以上两条性质的问题才适用动态规划思想。

### 3、动态规划问题代码实现思路？
> 动态规划主要有三种实现思路：
- 记忆型递归
- "人人为我"解法
- "我为人人"解法
> 其中递归容易爆栈，我为人人思路有点不符合现实逻辑，不宜想；
> 人人为我有点像是递归的形式，也有优化的可能，比较好。

####具体实现思路，以人人为我思想为主。
##### 1、人人为我
##### 2、记忆型递归
##### 3、我为人人

# 四 贪心算法
### 1、贪心算法定义
> 贪心算法（又称贪婪算法）是指，在对问题求解时，总是做出在当前看来是最好的选择。
> 也就是说，不从整体最优上加以考虑，他所做出的仅是在某种意义上的局部最优解。
> 贪心算法不是对所有问题都能得到整体最优解，但对范围相当广泛的许多问题他能产生整体最优解或者是整体最优解的近似解。

### 2、什么样的问题可以使用贪心？(贪心算法适用的情况很少，所以可以列举总结,记住就行了)
> 适合贪心问题的问题证明其实是很复杂的，
> 贪心策略适用的前提是：局部最优策略能导致产生全局最优解。
> 实际上，贪心算法适用的情况很少。一般，对一个问题分析是否适用于贪心算法，可以先选择该问题下的几个实际数据进行分析，就可做出判断。
> 因为用贪心算法只能通过解局部最优解的策略来达到全局最优解，
> 因此，一定要注意判断问题是否适合采用贪心算法策略，找到的解是否一定是问题的最优解。

### 3、贪心问题代码实现思路？
> 用贪心法设计算法的特点是一步一步地进行，常以当前情况为基础根据某个优化测度作最优选择，而不考虑各种可能的整体情况，
> 它省去了为找最优解要穷尽所有可能而必须耗费的大量时间，它采用自顶向下，以迭代的方法做出相继的贪心选择，
> 每做一次贪心选择就将所求问题简化为一个规模更小的子问题，通过每一步贪心选择，可得到问题的一个最优解，
> 虽然每一步上都要保证能获得局部最优解，但由此产生的全局解有时不一定是最优的，所以贪心法不需要回溯。

综上可知，
- 1、贪心算法适用性比较小，所以可以多见多记即可；
- 2、贪心算法也满足dp的两条性质，也就是贪心适用的dp也能实现，反之则不能确定；
        1、最优子结构；
        2、无后效性。

#### 基本思路

- ⒈、建立数学模型来描述问题。
- ⒉、把求解的问题分成若干个子问题。
- ⒊、对每一子问题求解，得到子问题的局部最优解。
- ⒋、把子问题的解局部最优解合成原来解问题的一个解。

#### 实现该算法的过程：

- 1、从问题的某一初始解出发；
- 2、while 能朝给定总目标前进一步 do
- 3、求出可行解的一个解元素；
- 4、由所有解元素组合成问题的一个可行解。

       从问题的某一初始解出发；
       while （能朝给定总目标前进一步）
      {
          利用可行的决策，求出可行解的一个解元素；
      }
      由所有解元素组合成问题的一个可行解；

# 五 回溯
### 1、回溯定义
> 概述：回溯法（探索与回溯法）是一种选优搜索法，按选优条件向前搜索，以达到目标。
> 但当探索到某一步时，发现原先选择并不优或达不到目标，就退回一步重新选择，
> 这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个状态的点称为“回溯点”。

#### 理解
> 回溯算法实际上一个类似枚举的搜索尝试过程，
> 主要是在搜索尝试过程中寻找问题的解，当发现已不满足求解条件时，就“回溯”返回，尝试别的路径。

> 回溯法是一种选优搜索法，按选优条件向前搜索，以达到目标。
> 但当探索到某一步时，发现原先选择并不优或达不到目标，就退回一步重新选择，
> 这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个状态的点称为“回溯点”。

> 许多复杂的，规模较大的问题都可以使用回溯法，有“通用解题方法”的美称。

### 2、什么样的问题可以使用回溯？
> 许多复杂的，规模较大的问题都可以使用回溯法，有“通用解题方法”的美称。

### 3、回溯问题代码实现思路？
#### 基本思想

``` html
      回溯法在问题的解空间树中，按深度优先策略，从根结点出发搜索解空间树。
      算法搜索至解空间树的任意一点时，先判断该结点是否包含问题的解。
        如果肯定不包含（剪枝过程），则跳过对该结点为根的子树的搜索，逐层向其祖先结点回溯；
        否则，进入该子树，继续按深度优先策略搜索。

      回溯法就是对隐式图的深度优先搜索算法

      回溯法：为了避免生成那些不可能产生最佳解的问题状态，
          要不断地利用限界函数(bounding function)来处死(剪枝)那些实际上不可能产生所需解的活结点，以减少问题的计算量。
          具有限界函数的深度优先生成法称为回溯法。（回溯法 = 穷举 +　剪枝）

      若用回溯法求问题的所有解时，要回溯到根，且根结点的所有可行的子树都要已被搜索遍才结束。

      而若使用回溯法求任一个解时而若使用回溯法求任一个解时，只要搜索到问题的一个解就可以结束。
```

#### 用回溯法解题的一般步骤：（穷举+深搜）

- 1）针对所给问题，定义问题的解空间:
      首先应明确定义问题的解空间，问题的解空间应至少包含问题的一个（最优）解。
- 2）确定易于搜索的解空间结构
- 3）以深度优先方式搜索解空间，并在搜索过程中用剪枝函数避免无效搜索。

两个常用的剪枝函数：

- (1)约束函数：在扩展结点处减去不满足约束的子数
- (2)限界函数：减去得不到最优解的子树

#### 算法框架
- 1）问题框架
      设问题的解是一个n维向量(a1,a2,………,an),约束条件是ai(i=1,2,3,…..,n)之间满足某种条件，记为f(ai)。
- 2）非递归回溯框架

> 用回溯法解题的一个显著特征是在搜索过程中动态产生问题的解空间。
> 在任何时刻，算法只保存从根结点到当前扩展结点的路径。
> 如果解空间树中从根结点到叶结点的最长路径的长度为h(n)，则回溯法所需的计算空间通常为O(h(n))。
> 而显式地存储整个解空间则需要O(2^h(n))或O(h(n)!)内存空间。

# 六 分支限定法
### 1、分支限定定义
> 类似于回溯法，也是一种在问题的解空间树T上搜索问题解的算法。但在一般情况下，分支限界法与回溯法的求解目标不同。
> `回溯法`的求解目标是找出T中满足约束条件的`所有解`，而`分支限界法`的求解目标则是找出满足约束条件的`一个解`，
> 或是在满足约束条件的解中找出使某一目标函数值达到`极大或极小的解`，即在某种意义下的`最优解`。

#### 分支搜索算法
> 所谓“分支”就是采用广度优先的策略，依次搜索E-结点的所有分支，也就是所有相邻结点，
> 抛弃不满足约束条件的结点，其余结点加入活结点表。
> 然后从表中选择一个结点作为下一个E-结点，继续搜索。

选择下一个E-结点的方式不同，则会有几种不同的分支搜索方式。
- 1）FIFO搜索
- 2）LIFO搜索
- 3）优先队列式搜索

### 2、什么样的问题可以使用分支限定法？
> 类似于回溯法，也是一种在问题的解空间树T上搜索问题解的算法。但在一般情况下，分支限界法与回溯法的求解目标不同。
> `回溯法`的求解目标是找出T中满足约束条件的`所有解`，而`分支限界法`的求解目标则是找出满足约束条件的`一个解`，
> 或是在满足约束条件的解中找出使某一目标函数值达到`极大或极小的解`，即在某种意义下的`最优解`。

### 3、分支限定法代码实现思路？

> 分支限界法常以广度优先或以最小耗费（最大效益）优先的方式搜索问题的解空间树。
> 在分支限界法中，每一个活结点只有一次机会成为扩展结点。活结点一旦成为扩展结点，就一次性产生其所有儿子结点。
> 在这些儿子结点中，导致不可行解或导致非最优解的儿子结点被舍弃，其余儿子结点被加入活结点表中。
> 此后，从活结点表中取下一结点成为当前扩展结点，并重复上述结点扩展过程。这个过程一直持续到找到所需的解或活结点表为空时为止。

#### 分支限界法与回溯法的区别：
- （1）求解目标：
      回溯法的求解目标是找出解空间树中满足约束条件的所有解，
      而分支限界法的求解目标则是找出满足约束条件的一个解，或是在满足约束条件的解中找出在某种意义下的最优解。
- （2）搜索方式的不同：
      回溯法以深度优先的方式搜索解空间树，
      而分支限界法则以广度优先或以最小耗费优先的方式搜索解空间树。

#### 常见的两种分支限界法：
- （1）队列式(FIFO)分支限界法
      按照队列先进先出（FIFO）原则选取下一个节点为扩展节点。
- （2）优先队列式分支限界法
      按照优先队列中规定的优先级选取优先级最高的节点成为当前扩展节点。
#### 例子：单源最短路径问题
- 1、问题描述
      在下图所给的有向图G中，每一边都有一个非负边权。要求图G的从源顶点s到目标顶点t之间的最短路径。

![1](https://hunzino1.github.io/assets/images/2018/algorithm_summary/lowest1.jpg)

      下图是用优先队列式分支限界法解有向图G的单源最短路径问题产生的解空间树。
      其中，每一个结点旁边的数字表示该结点所对应的当前路长。

![2](https://hunzino1.github.io/assets/images/2018/algorithm_summary/lowest2.jpg)

      找到一条路径：

![3](https://hunzino1.github.io/assets/images/2018/algorithm_summary/lowest3.jpg)

      目前的最短路径是8，一旦发现某个结点的下界不小于这个最短路进，则剪枝：

![4](https://hunzino1.github.io/assets/images/2018/algorithm_summary/lowest4.jpg)

      同一个结点选择最短的到达路径：

![5](https://hunzino1.github.io/assets/images/2018/algorithm_summary/lowest5.jpg)
![6](https://hunzino1.github.io/assets/images/2018/algorithm_summary/lowest6.jpg)

- 2.剪枝策略

```html
    在算法扩展结点的过程中，一旦发现一个结点的下界不小于当前找到的最短路长，则算法剪去以该结点为根的子树。
    在算法中，利用结点间的控制关系进行剪枝。从源顶点s出发，2条不同路径到达图G的同一顶点。
    由于两条路径的路长不同，因此可以将路长长的路径所对应的树中的结点为根的子树剪去。
```

- 3.算法思想

```html
    解单源最短路径问题的优先队列式分支限界法用一极小堆来存储活结点表。其优先级是结点所对应的当前路长。
    算法从图G的源顶点s和空优先队列开始。结点s被扩展后，它的儿子结点被依次插入堆中。
    此后，算法从堆中取出具有最小当前路长的结点作为当前扩展结点，并依次检查与当前扩展结点相邻的所有顶点。
    如果从当前扩展结点i到顶点j有边可达，且从源出发，途经顶点i再到顶点j的所相应的路径的长度小于当前最优路径长度，
    则将该顶点作为活结点插入到活结点优先队列中。
    这个结点的扩展过程一直继续到活结点优先队列为空时为止。
```
