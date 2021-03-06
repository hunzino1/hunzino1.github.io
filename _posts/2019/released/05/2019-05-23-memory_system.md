---
layout:     post
title:      内存系统1 - 内存初窥
category: stack
tags: [stack]
excerpt: 一个人有多努力，就会有多幸运。
---

内存系统1 - 内存初窥
=======================================

内存系统，印象中已经看过不止一次，学习epoll时看过，页缓存时看过，为什么一遍一遍的浪费精力，其实就是没有静下心来，

踏踏实实的将内存机制精通。

精通一个领域，既能避免重复的无意义时间浪费，又能成为自己的一个闪光点，win-win的举动。

通过本系列博客，我将会对内存系统进行系统的学习，过时的一些方案浅尝辄止，主要是对目前正在使用的内存机制进行总结。

当然，更多精彩可以见《鸟哥私房菜》，更加系统的学习。

-------------------------------------

1 大佬博客
------------------------------------

[基本内存管理](https://www.cnblogs.com/edisonchou/p/5090315.html)

[分页内存管理](https://www.cnblogs.com/edisonchou/p/5094066.html)

[段式内存管理](https://www.cnblogs.com/edisonchou/p/5115242.html)

以上上博客园一大神博客，仔细研读之后，认为对内存机制的阐述比较清晰全面，所以感觉不必浪费时间再去总结一把了。

入门已经够用了，如果想要深入了解，则需要研读《鸟哥》等大佬数据，去专门学习了。

在此只是对自己研读收获做一总结。

2 博客总结
--------------------------------------

### 2.1 博客脉络

如上，三篇博客按照 基本内存管理、分页内存管理、段式内存管理 三段式对计算机内存机制进行梳理，

从问题到解决方案展开探讨，以下只是自己对博客的总结以及自己的一些理解。

#### 2.1.1 基本内存管理

1 **操作系统(OS)的两个重要角色：CPU 和 内存**

```html
    OS内存管理的目的：
      1、地址保护：一个程序不能访问另一个程序地址空间。
　　  2、地址独立：程序发出的地址应该与物理主存地址无关。

    虚拟内存： 内存 + 硬盘的假设内存
      所以实际使用中，需要将硬盘中的数据交换到内存中，OS才能真正处理，此过程叫内存交换；
      内存交换过于频繁，OS处理速度就会下降甚至卡顿、停止响应，这种现象称为抖动（Thrushing）；
```

2 **基本内存管理**

```html'
    1 单道编程的内存管理
      整个内存里面只有两个程序：一个是用户程序，另一个是操作系统。
      静态地址翻译: 将用户程序总是加载到同一个内存起始地址，那么在程序运行之前就计算出所有的物理地址。

    2 多道编程的内存管理
      因为有多个用户程序，所以只能在程序加载完毕之后才能计算物理地址，也就是在程序运行时进行地址翻译，这种翻译称为动态地址翻译。
      需要使用地址翻译器(MMU)将虚拟地址匹配到物理内存地址。
```

3 **多道编程内存管理策略**

这部分可以看博客，分 固定分区 和 非固定分区两种。

重点是非固定分区，这是页式内存管理的基础实现，但是存在**空间浪费(碎片化) 和 程序大小受限**

**而页式内存管理就是解决了上述问题的一种非固定分区策略。**

4 **闲置空间管理**

闲置空间管理分 位图表示法 和 链表表示法 两种，很好理解。

#### 2.1.2 分页内存管理

1 **虚拟地址的构成与地址翻译**

虚拟地址的组成： 页面号 和 页内偏移量

页面号是用来定位物理内存页面(映射), 页内偏移量 是确定页中的数据起始位置。

下图很形象：

![address_map](https://hunzino1.github.io/assets/images/2019/os/address_map.jpg)

2 **页表**

> 优缺点看博客

```HTML
    1 页表是由硬件提供
    2 页表记录数 = 虚拟地址数目
    3 虚拟地址通过 查页表 定位物理内存
```

3 **缺页中断** (看博客)

##### 4 页面置换算法

原因： 如果发生了缺页中断，就需要从磁盘上将需要的页面调入内存。如果内存没有多余的空间，就需要在现有的页面中选择一个页面进行替换。

目的： 页面置换时挑选页面的目标主要在于降低随后发生缺页中断的次数或概率。

算法见博客，这里只说一下工作集算法。

1. 随机更换算法
2. 先进先出算法
3. 第二次机会算法
4. 时钟算法
5. NRU(最近最少使用算法)
6. [LRU(最近最少使用算法)](https://hunzino1.github.io/better/2019/04/29/LRU_Cache.html)
7. 工作集算法
8. 工作集时钟算法

```html
    工作集算法：
      由于不可能精确地确定那个页面是最近最少使用的，
      那就干脆不花费这个力气，
      只维持少量的信息使得我们能够选出的替换页面是不太可能是马上又会使用的页面即可。

      这种少量的信息就是工作集信息。


    原理：
      工作集的基础原理是一个程序在某个时间访问的空间是有限的，即时空有限性；
      比如最近k次访问(时间段)均发生在某m个页面上，那么m就是参数为k时的工作集。

      我们用w(k,t)来表示在时间t时k次访问所涉及的页面数量。

    工作集和最近访问次数的关系见博客图片链接。

    结论：
      如果一个程序在内存里面的页面数与其工作集大小相等或者超过工作集，则该程序可在一段时间内不会发生缺页中断。
      如果其在内存的页面数小于工作集，则发生缺页中断的频率将增加，甚至发生内存抖动。

      所以，工作计算法的目标就是维持当前的工作集的页面在物理内存里面。每次页面更换时，寻找一个不属于当前工作集的页面替换即可。
      重点： 当前工作集

    工作集时钟算法： 更加公平，工作集算法由于其数据结构是线性的，会造成每次都按同样的顺序进行扫描，显得不太公平。
      
```

#### 2.1.3 段式内存管理

分页系统存在的一个无法容忍，同时也是分页系统无法解决的一个缺点就是：一个进程只能占有一个虚拟地址空间。

```html
猜测：

  一个进程只能占有一个虚拟地址空间很好理解，就是只分配一个起始地址。

```

而分段管理就是先将程序分多段，那么这多段程序都可以有单独的地址空间，像博客图中按照变量、代码等分为了五个段，

这样每一个段都有对应的地址空间，段之间不会发生碰撞，各自按需调整内存，内存管理更加灵活。

段页式内存管理的实现包括段表和页表，每个段都有对应的页式内存管理。

3 总结
-----------------------------------------

至此，博客已总结完毕，不过这只是基础，还有很多需要深入学习，加油！
