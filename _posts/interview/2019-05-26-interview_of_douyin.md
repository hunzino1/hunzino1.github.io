---
layout:     post
title:      抖音面试准备
category: interview
tags: [interview]
excerpt: 一个人有多努力，就会有多幸运。
---

抖音面试准备
=======================================

2019-05-27就要去抖音面试了，不知道以后的几几年世界会怎么样，但是2019年抖音肯定是最火的app之一

所以希望有好运给我

我要的不多，只是想变牛逼，然后一切水到渠成

阿门！！！

----------------------------------------------------------

1 [抖音后台开发社招面试](https://www.cnblogs.com/xuwangzihao/p/9484058.html)
--------------------------------------------------------------------------------

### 1.1 一面 

- 你面临过的最大的问题是什么

```html
三、遇到过的最大的挑战是什么，怎么克服的？

这个问题很有挑战性，主要是考察工作中解决问题的能力，
如果一个人连最基本的挑战都想不起来，那说明你在旧环境中的工作状态没有进入到最佳，没有突破自我。
要回答这个问题，不仅可以列举工作上的挑战，还可以列出自己心理上的挑战，
只要你心理上有一个巨大的转变的，就是很好的挑战。最好能以举出例子。

裁员之后很多核心业务
```

- 100Mbps的带宽三个人使用，每人50Mbps，tcp怎么保证速度的

[TCP协议-如何保证传输的高性能](https://blog.csdn.net/liuchenxia8/article/details/80434264)

[为什么多 TCP 连接比单TCP连接传输快](https://segmentfault.com/a/1190000008803687?utm_source=tag-newest)

```html
滑动窗口
快速重传
延迟应答
捎带应答
```

- tcp四次挥手具体的东西

[传送门](https://hunzino1.github.io/interview/2019/01/18/interview_tcp.html)


- 进程和线程的区别
- 进程间通信的方法都有什么

[面试总结](https://hunzino1.github.io/interview/2019/03/26/better_little_01.html)

- CtrlC可以终止程序，我们是否可以在程序终止的时候做一些别的操作？

设置一个回调函数，强制停止的时候触发操作；

- 1000qps单机怎么处理？

每秒查询率QPS是对一个特定的查询服务器在规定时间内所处理流量多少的衡量标准。

QPS = 并发量 / 平均响应时间

- 算法题：一个超级长的数字（有几十位那种），允许进行任意多次的任意两位交换，问你能得到的比他大的最小的数字是多少。

[字典排序](https://hunzino1.github.io/interview/2019/03/26/better_little_01.html)

### 1.2 二面

- 为什么想来抖音
- nginx的location是什么

[详解Nginx Location配置](https://www.jb51.net/article/140214.htm)

[对nginx中location的认识](https://www.cnblogs.com/jingxiaoniu/p/7007653.html)

- 算法题：实现一个LFUcache，要求实现set和get方法，复杂度为O(1)。（该题的LFUcache是一个存储空间，有容量，是整数n，存key-value对，每访问一次这个kv对的频率就加一，如果新set kv对 的时候，已经达到容量上限，删除频率最低的kv对。）

[LFU缓存](https://blog.csdn.net/Aphysia/article/details/77884269)

[LeetCode算法题解：LFU Cache](https://www.jianshu.com/p/437f53341f67)

- http状态码501，502，503，504都什么意思

[服务器常见错误代码500、501、502、503、504、505](https://www.cnblogs.com/lgp2000/p/6873930.html)

- pyc是什么，什么时候生成

[pyc是什么？](https://blog.csdn.net/thanklife/article/details/80693267)

[Ruby 画说 Ruby 与 Python 垃圾回收](https://ruby-china.org/topics/28127?page=2)

- http和https的区别，为什么项目不能一股脑从http升级为https，有什么需要注意的地方（为什么有的项目不能升级到https）。

[1111](https://www.zhihu.com/question/310263956?sort=created)

- 操作系统进程状态都有什么

进程状态反映进程执行过程的变化。这些状态随着进程的执行和外界条件的变化而转换。在三态模型中，进程状态分为三个基本状态，即运行态，就绪态，阻塞态。在五态模型中，进程分为新建态、终止态，运行态，就绪态，阻塞态。


- 简述分页分段机制

[内存系统1 - 内存初窥](https://hunzino1.github.io/stack/2019/05/23/memory_system.html)

页面号 + 偏移值 + 页表 + 缺页中断 + 页面置换算法 + 段式内存管理

2 [前端](https://blog.csdn.net/lj1934197878/article/details/85052349)
----------------------------------------------------------------------------

4.给一个字符串，找出出现次数最多的字符和对应的次数。（编程）

计数排序

5.最大子序和，时间复杂度O（n）

5.写一个函数，实现深拷贝，要考虑所有数据类型

[ruby](https://t284299773.iteye.com/blog/1216169)

http

1.与缓存相关的header有哪几个？分别是什么内容？（last-Modified之类的）如果是last-modified，服务器查询了下，没有更新数据，会给客户端返回哪个码？

[http中和缓存相关的header](https://blog.csdn.net/weixin_34127717/article/details/88841546)

Expires cache-control last-modify etags

2.http是基于什么协议的？TCP三次握手与四次挥手的过程。

TCP协议

3.dns解析的过程

[dns](https://hunzino1.github.io/interview/2019/04/15/interview2.html)

4.什么情况下属于跨域？跨域的解决方式（jsonp了解下）

[jsonp](https://blog.csdn.net/u011897301/article/details/52679486)

5.说一下cookie与session。cookie添加到了http请求的哪里，设置什么字段就可以携带cookie?

当服务器收到HTTP请求时，服务器可以在响应头里面添加一个Set-Cookie选项。浏览器收到响应后通常会保存下Cookie，之后对该服务器每一次请求中都通过Cookie请求头部将Cookie信息发送给服务器。

httpGet.addHeader("Cookie", cookieStore.toString());

6.从用户点击注册开始，都发生了什么

3 第一面问些基础知识，包括操作系统，软件基础，redis等等。第二面就是架构方面的知识，分布式的高可用方案等等。

4 考了闭包(js)

第一题是45度打印二维数组

3 [抖音后端](https://blog.csdn.net/ChenRui_yz/article/details/88223204)
---------------------------------------------------------------------

一面（1小时）
hashmap，怎么扩容，怎么处理数据冲突？怎么高效率的实现数据迁移？

```html
(1) 扩容是一个特别耗性能的操作，所以当程序员在使用HashMap的时候，估算map的大小，初始化的时候给一个大致的数值，避免map进行频繁的扩容。

(2) 负载因子是可以修改的，也可以大于1，但是建议不要轻易修改，除非情况非常特殊。

(3) HashMap是线程不安全的，不要在并发的环境中同时操作HashMap，建议使用ConcurrentHashMap。

(4) JDK1.8引入红黑树大程度优化了HashMap的性能。

(5) HashMap的性能提升仅仅是JDK1.8的冰山一角。
```

Linux的共享内存如何实现，大概说了一下。

[1](https://www.cnblogs.com/liunianshiwei/p/6110384.html)

管道，消息队列，信号量等

[总结](https://hunzino1.github.io/interview/2019/03/26/better_little_01.html)

同步IO和异步IO的区别？

[11111](https://www.2cto.com/kf/201611/561895.html)

Java GC机制？GC Roots有哪些？
红黑树讲一下，五个特性，插入删除操作,时间复杂度？

[1111](https://hunzino1.github.io/algorithm/2019/05/03/red_black_tree.html)

快排的时间复杂度，最坏情况呢，最好情况呢，堆排序的时间复杂度呢，建堆的复杂度是多少

堆排序的时间复杂度是O(n log n),堆排序中建堆过程的时间复杂度是O(n)

链接：https://www.nowcoder.com/questionTerminal/385157a6b64540c3b233bd12f8a83ee7?pos=328&mutiTagIds=134&orderByHotValue=1
来源：牛客网

堆排序的时间，主要由建立初始堆和反复重建堆这两部分的时间开销构成，它们均是通过调用Heapify实现的。时间复杂度O(n x logn)

如果从底部最后的父节点开始建堆，那么我们可以大概算一下：

假如有N个节点，那么高度为H=logN，最后一层每个父节点最多只需要下调1次，倒数第二层最多只需要下调2次，顶点最多需要下调H次，而最后一层父节点共有2^(H-1)个,倒数第二层公有2^(H-2),顶点只有1(2^0)个，所以总共的时间复杂度为s = 1 * 2^(H-1) + 2 * 2^(H-2) + ... + (H-1) * 2^1 + H * 2^0 将H代入后s= 2N - 2 - log2(N)，近似的时间复杂度就是O(N)。


二面（1小时）
自我介绍，主要讲讲做了什么和擅长什么
设计模式了解哪些？

AtomicInteger怎么实现原子修改的？

ConcurrentHashMap 在Java7和Java8中的区别？为什么Java8并发效率更好？什么情况下用HashMap，什么情况用ConcurrentHashMap？

[11111](https://segmentfault.com/a/1190000016066203?utm_source=tag-newest)

```html
JDK1.7中HashMap采用了数组+链表的数据结构，有线程安全问题（统计不准确，丢失数据，死循环cpu100%），1.8中采用了数组+链表+红黑树的结构，有线程安全问题（统计不准确，丢失数据）。1.8中优化了hash算法，并且每次扩容不需要重新计算hash值。
JDK1.7中concurentHashMap使用了segment保证线程安全，但是在1.8中又把它优化掉了，直接使用cas+synchronized
```

redis数据结构？

redis数据淘汰机制？

```html
区分不同的淘汰策略选择不同的key，主要分为随机淘汰、LRU淘汰、TTL时间淘汰

随机淘汰的场景下获取待删除key的策略，随机找hash桶再次hash指定位置的dictEntry即可。

LRU 策略淘汰思路如下：

dictGetRandomKeys随机获取指定数目的dictEntry。
将获取的的dictEntry进行下sort按照最近时间进行排序。
选择最近使用时间最久远的数据进行过期
每次过期的数据其实是采样的结果数据中的最近未被访问数据而非全局的。

TTL时间淘汰策略跟随机策略很像，唯一的区别就是TTL时间淘汰基于采样结果进行选择然后选择距离过期时间最近的数据进行过期，所以他理论上结合了采样+TTL时间计算进行数据淘汰的。
```


三面（约五十分钟）
mysql实现事务的原理(MVCC)

[1111](https://www.cnblogs.com/yxh168/p/9174932.html)

[我的](https://hunzino1.github.io/interview/2019/04/22/interview7.html)

```
多版本和快照隔离(mvcc)

    MVCC是行级锁的一个变种,但是它在很多情况下避免了加锁操作,因此开销更低,虽然实现机制有所不同,但大都实现了非阻塞的读操作,写操作也只锁定必要的行.

    MVCC的实现是通过保存数据在某个时间点的快照来实现的,也就是说,不管需要执行多长时间,只要事务开始时间相同,每个事务看到的数据都是一致的,事务开始的时间不同时,每个事务对同一张表,同一时刻看到的数据可能是不一样的(因为不同的时间点可能数据就已经产生了不同的快照版本，而每个事务在默认的RR隔离级别下只能看到事务开始时的数据快照)
```

MySQL数据主从同步是如何实现的？

[111](https://www.cnblogs.com/kylinlin/p/5258719.html)

```
通过增加从服务器来提高数据库的性能，在主服务器上执行写入和更新，在从服务器上向外提供读功能，可以动态地调整从服务器的数量，从而调整整个数据库的性能。
提高数据安全-因为数据已复制到从服务器，从服务器可以终止复制进程，所以，可以在从服务器上备份而不破坏主服务器相应数据
在主服务器上生成实时数据，而在从服务器上分析这些数据，从而提高主服务器的性能


mysql是异步复制的，而MySQL Cluster是同步复制的。有很多种主从同步的方法，但核心的方法有两种，Statement Based Replication（SBR）基于SQL语句的复制，另一种是Row Based Replication（RBR）基于行的复制，也可以使用Mixed Based Replication（MBR）。在mysql5.6中，默认使用的是SBR。而mysql 5.6.5和往后的版本是基于global transaction identifiers(GTIDs)来进行事务复制。当使用GTIDs时可以大大简化复制过程，因为GTIDs完全基于事务，只要在主服务器上提交了事务，那么从服务器就一定会执行该事务。
```

MySQL索引的实现，innodb的索引，b+树索引是怎么实现的，为什么用b+树做索引节点，一个节点存了多少数据，怎么规定大小，与磁盘页对应。

主存和磁盘以页为单位交换数据，将一个节点的大小设为等于一个页，因此每个节点只需一次I/O就可以完全载入。

如果Redis有1亿个key，使用keys命令是否会影响线上服务？

[keys](https://blog.csdn.net/ouyang111222/article/details/50270097)

开发中使用keys的模糊匹配却发现redis的CPU使用率极高，所以公司的redis生产环境将keys命令禁用了！

那怎么解决这种类似的keys模糊匹配问题呢？其中常见的方法就是设置一个set，将需要使用的keys存储在set中


Redis的持久化方式，aod和rdb，具体怎么实现，追加日志和备份文件，底层实现原理的话知道么?

AOF采用文件追加模式，文件会越来越大，为了避免这种情况，新增了重写机制，当AOF文件的大小超过指定的阈值时，redis会启动AOD内容压缩，只保留可以恢复数据的最小指令集；

rdb：在指定的时间间隔内将数据快照写入磁盘，恢复时将快照文件直接写入内存

[1111](https://blog.csdn.net/qq_30325833/article/details/89644514)

遇到最大困难是什么？怎么克服？
未来的规划是什么？
你想问我什么？

4 truncate 和 delete 只删除数据不删除表的结构(定义) 　　drop 语句将删除表的

[1111](https://www.cnblogs.com/jun9207/p/5035736.html)

4 [2018-07-26 抖音面试题汇总](https://blog.csdn.net/weixin_33841722/article/details/86821177)
-------------------------------------------------------------------------------------------------

互斥锁、自旋锁、递归锁啥区别？

[111](https://blog.csdn.net/qq100440110/article/details/51076609)

都是保证只有一个线程访问对象的锁。互斥会睡眠线程，自选会循环访问，递归是特殊的互斥，特点是允许反复对锁加锁。

https的过程

[1111](https://www.cnblogs.com/liyuhui-Z/p/7844880.html)

手写反转二叉树

什么情况下 快速排序最慢
在处理有序数组的时候排序最慢

[抖音/火山 后台开发三面 捞offer](https://www.nowcoder.com/discuss/148961?type=0&order=3&pos=10&page=1)

【一面】
- 0、自我介绍 （之前有过实习经历，主要写python和go balabala
- 1、get post的区别 put delete 知道吗 put和post  （安全 幂等 长度 状态balabala）

url 长度限制

[11111](https://www.cnblogs.com/logsharing/p/8448446.html)

并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次。

```
PUT请求：如果两个请求相同，后一个请求会把第一个请求覆盖掉。（所以PUT用来改资源）

Post请求：后一个请求不会把第一个请求覆盖掉。（所以Post用来增资源）

DELETE方法：对这个资源的删操作。但要注意：客户端无法保证删除操作一定会被执行，因为HTTP规范允许服务器在不通知客

户端的情况下撤销请求。

```

- 2、innodb为什么用b+树 ？多路树的好处？（说了下比较好控制高度 查询稳定 又说了一下对比b树的优势balabala）

    为什么控制高度？（连续读磁盘 效率高）

    详细描述b+？（以innodb索引为例给他画了下）

- 3、tcp为什么三次连接 二次 四次？

- 4、innodb 数据隔离级别 （四个隔离级别说了下 顺带把脏读 幻读 不可重复度说了下）

- 5、设计题 一个高并发定时执行任务：实现一个方法 接受一个任务以及它要开始执行的时间，定时执行这些任务，会有很多任务（设计了个数据结构用空间换时间，后来又问有没有别的方法，我当时脑袋一抽说要不就开goroutine，不过任务太多就爆了。后来回去想想 可以用优先队列）

优先队列

有序集合

- 6、redis （问了些基础

- 7、输入一个url 整个过程 返回结果和渲染是同时的吗 （我从七层分别说了下 又问我返回结果和渲染是同时的吗 ，我说不是 然后扯到http2.0服务端推送上

[11111](https://blog.csdn.net/qq_37337830/article/details/81512625)

8、http 1.X 2.0区别 （ 帧 流 推送 头部压缩 安全性等等 ，答得有些乱

[111111111](https://www.cnblogs.com/frankyou/p/6145485.html)

9、链表 奇位上升偶位下降 整合成升序链表 （非常友好的题了


【二面】
1、LRU实现、插入操作、 描述数据结构如何变化 （说双向链表加哈希，在双向链表上做lru，加哈希表是为了快速定位要移动的节点）

[LRU](https://hunzino1.github.io/better/2019/04/29/LRU_Cache.html)

2、实现哈希表 冲突过多的时候如何解决 （扩容

3、redis zset 数据结构描述 （之前看过黄建宏的redis设计与实现原理（强推，还有redis实战）,答得还比较轻松，zset是一个字典加跳表 又详细描述（画）了下跳表

4、大数加法 链表 （也很友好了

5、zset 除了用跳表 还可以用什么实现 （想了想 可以红黑树，不过说完我就赶紧说 可是红黑树我不会实现，提前认怂，面试官就笑了，他说不用实现别怕）

如何用红黑树如何实现zrange by score （我第一反应是加个双向链表，但是没有想好到底怎么和红黑树连起来 。后来面试官提示，可以在红黑树里加索引）


【三面】
1、Tcp: 拔网线之后连接是否存在 为什么  （记得tcp的长连接是有一个类似心跳检测的机制,忘了叫啥了，面试官问我心跳检测是在传输层吗还是应用层 ，我说应用层有心跳检测，但tcp那层也有类似的，后来回来看了下tcp的保活，跟我当时说的差不多，就是名词没想起来）

[1111](https://blog.csdn.net/larry_zeng1/article/details/78437050)

2、联合索引：b+树是什么状态 （画了一下，面试官反复问我高度什么的，我说高度没有影响，说了最左前缀 ）

[联合索引在B+树上的结构](https://blog.csdn.net/weixin_30531261/article/details/79329722)

[1111](https://blog.csdn.net/zgjdzwhy/article/details/84062105)

3、寻找中位数 （ 堆

4、一棵树 寻找节点中最长路径 （动规

5、sql语句

6、 操作系统如何识别tcp连接 （问懵了 答的不好

netstat 可以查看


[头条后端实习面经(抖音)](https://www.nowcoder.com/discuss/151613)

线程和进程的区别（真的必考）

ThreadLocal

四种引用类型

volatile的作用和原理

juc里的锁

atomic包里的一些问题

二叉树的先序遍历，层序遍历的实现

用栈实现队列

包括max函数的栈

一面问的比较简单，我答得还凑合，很快就约二面了，当时hr听说我要期末考试，要给我延后面试，感觉人非常nice

1月14号下午约的视频面，面了三轮技术面

第一轮（有一些不太记得了）：

简述TCP的三次握手，为什么要三次握手？

TCP的拥塞控制

[11111](https://hunzino1.github.io/interview/2019/04/22/interview7.html)

redis分片，客户端的请求怎么处理

[就是分区](https://www.cnblogs.com/shiw27/p/8360485.html)

手写代码，滑动窗口的最大值

[剑指offer]


第二轮：

浏览器输入url请求服务器的过程，分析其中哪些部分用到缓存

[1](https://blog.csdn.net/jxian6g/article/details/52910531)

域名 

cookie

js css缓存

讲一下CDN

[1111](https://hunzino1.github.io/interview/2019/04/22/interview7.html)

cookie用来做什么的

操作系统的内存管理

分页式的页表放在哪

进程的PCB里还有哪些东西

[1111](https://blog.csdn.net/wannerwang/article/details/49207395)

进程控制块中的信息

1：进程标识符（PID）：用于唯一标识一个进程一个进程通常有两种标识符1：外部与内部标识符。

2：处理机状态：处理机的状态信息，也称为处理机的上下文，当进程切换时，处理机的状态信息必须保存在相应的PCB中，以便在该进程重新执行时从断点处开始。

3：进程调度信息：在OS进行进程调度的时候，必须了解进程的状态以及进程的调度信息。

如（进程的状态，进程的优先级，其他消息（进程调度算法），事件（引起阻塞原因））

4：进程的控制信息：包括1：程序和数据地址，2：进程同步和通信机制，3：资源抢清单4：连接指针。

linux脚本，杀掉包含一个关键字的所有进程

```html
ps -ef|grep ./amplxe-gui|grep -v grep|cut -c 9-15|xargs kill -9

批量杀死包含关键字“./amplxe-gui”的进程。

"ps -ef" ——查看所有进程

"grep ./amplxe-gui" ——列出所有含有关键字"./amplxe-gui"的进程

"grep -v grep" ——在列出的进程中去除含有关键字"grep"的进程（因为我们在前一步生成的grep进程也包含关键字）

"cut -c 9-15" ——截取输入行的第9个字符到第15个字符，而这正好是进程号PID

"xargs kill -9" ——xargs 命令是用来把前面命令的输出结果（PID）作为"kill -9"命令的参数，并执行该命令。"kill -9"会强行杀掉指定进程。
```

linux I/O模型，说说select和epoll的区别

区别（epoll相对select优点）主要有三：

```
1.select的句柄数目受限，在linux/posix_types.h头文件有这样的声明：#define __FD_SETSIZE    1024  表示select最多同时监听1024个fd。而epoll没有，它的限制是最大的打开文件句柄数目。

2.epoll的最大好处是不会随着FD的数目增长而降低效率，在selec中采用轮询处理，其中的数据结构类似一个数组的数据结构，而epoll 是维护一个队列，直接看队列是不是空就可以了。epoll只会对"活跃"的socket进行操作---这是因为在内核实现中epoll是根据每个fd上面 的callback函数实现的。那么，只有"活跃"的socket才会主动的去调用 callback函数（把这个句柄加入队列），其他idle状态句柄则不会，在这点上，epoll实现了一个"伪"AIO。但是如果绝大部分的I/O都是 “活跃的”，每个I/O端口使用率很高的话，epoll效率不一定比select高（可能是要维护队列复杂）。

3.使用mmap加速内核与用户空间的消息传递。无论是select,poll还是epoll都需要内核把FD消息通知给用户空间，如何避免不必要的内存拷贝就很重要，在这点上，epoll是通过内核于用户空间mmap同一块内存实现的。


支持的fd数量有限：
单个进程能够监视的文件描述符的数量存在最大限制，通常是1024，当然可以更改数量，但由于select采用轮询的方式扫描文件描述符，文件描述符数量越多，性能越差；(在linux内核头文件中，有这样的定义：#define单个进程能够监视的文件描述符的数量存在最大限制，通常是1024，当然可以更改数量，但由于select采用轮询的方式扫描文件描述符，文件描述符数量越多，性能越差；(在linux内核头文件中，有这样的定义：#define
__FD_SETSIZE 1024)

每次调用select, 都会把fd从用户态拷贝到内存态 ；
内核 / 用户空间内存拷贝问题，内核 / 用户空间内存拷贝问题，select需要复制大量的句柄数据结构，产生巨大的开销；

使用的是轮询的方式，需要内核遍历传进来的所有fd;
select返回的是含有整个句柄的数组，应用程序需要遍历整个数组才能发现哪些句柄发生了事件；select返回的是含有整个句柄的数组，应用程序需要遍历整个数组才能发现哪些句柄发生了事件；

对于第一个缺点，fd数目有限：
　　epoll没有这个限制，它所支持的FD上限是最大可以打开文件的数目，这个数字一般远大于2048,举个例子,在1GB内存的机器上大约是10万左右，具体数目可以cat /proc/sys/fs/file-max查看,一般来说这个数目和系统内存关系很大。
　　
对于第二个缺点，重复把fd从用户态拷贝到内存态：
epoll的解决方案在epoll_ctl函数中。每次注册新的事件到epoll句柄中时（在epoll_ctl中指定EPOLL_CTL_ADD），会把所有的fd拷贝进内核，而不是在epoll_wait的时候重复拷贝。epoll保证了每个fd在整个过程中只会拷贝一次。

对于第三个缺点，轮询方式遍历所有的fd：
　　epoll的解决方案不像select那样每次都把current轮流加入fd对应的设备等待队列中，而只在epoll_ctl时把current挂一遍（这一遍必不可少）并为每个fd指定一个回调函数，当设备就绪，唤醒等待队列上的等待者时，就会调用这个回调函数，而这个回调函数会把就绪的fd加入一个就绪链表）。epoll_wait的工作实际上就是在这个就绪链表中查看有没有就绪的fd（利用schedule_timeout()实现睡一会，判断一会的效果，和select实现中的第7步是类似的）。
```

手写代码，找一个n x n矩阵中的最长上升序列


[抖音三套](http://youzhixueyuan.com/toutiao-java-3-round-interview-questions.html)

5 [字节跳动后台开发岗三轮技术面试分享总结](https://blog.csdn.net/weixin_43932886/article/details/86622069)
