---
layout:     post
title:      计算机网络 - 中断
category: stack
tags: [stack]
excerpt: 一个人有多努力，就会有多幸运。
---

计算机网络 - 中断
=======================================

[参考一下](https://blog.csdn.net/zhangskd/article/details/21992933)

-----------------------------------

1 中断
-------------------------

### 1 概念
> 中断： 操作系统终止当前处理过程的一个信号。

中断是什么很好理解，但在操作系统中中断是一个过程，面试可能需要描述这个过程。

面试回答：

中断是一个信号，由硬件或者软件产生中断信号，从而终止cpu当前正在运行的程序，转到发生中断事件的程序中。

### 2 中断的作用：

解放了CPU，提高了CPU的执行效率，如果没有中断技术，cpu会一直等在某一个IO想要，一直阻塞下去。

中断控制的主要优点是只有在IO接口需要服务时才去响应它，使得CPU实现调度。同时中断中也设计了中断优先级，来处理一些很紧急的事件。

### 3 中断的物理过程

#### 1 过程

中断电信号 -->  终端控制器 --> 发CPU信号 --> cpu终止作业处理中断 --> cpu通知操作系统 --> os处理中断

```html
其实从 物理学的角度看，中断是一种电信号，由硬件设备产生，并直接送入中断控制器（如 8259A）的输入引脚上，然后再由中断控制器向处理器发送相应的信号。处理器一经检测到该信号，便中断自己当前正在处理的工作，转而去处理中断。此后， 处理器会通知 OS 已经产生中断。这样，OS 就可以对这个中断进行适当的处理。不同的设备对应的中断不同，而每个中断都通过一个唯一的数字标识，这些值通常被称为中断请求线。
```

#### 2 那么 当产生一个中断时，CPU是如何识别的呢？

在Intel X86中可以支持256种向量中断，为了使处理器能使别每种中断源，给它们进行了编号----->叫做中断向量

#### 3 这些中断向量在Linux中是如何分配的：

编号0～31的向量对应于异常和非屏蔽中断

编号32～47的向量（即由IO设备引起的中断）分配给屏蔽中断。

编号48～255的向量用来标示软中断。Linux用其中的128或0x80来实现系统调用

非屏蔽中断的向量和异常的向量是固定的。

#### 4 异常和中断的区别：

1. 异常：是指CPU内部出现的中断，即在CPU执行特定指令时出现的非法情况。同时异常也称为同步中断，因此只有在一条指令执行后才会发出中断 ，不可能在指令执行期间发生异常。

a. 产生的原因：

    程序的错误产生的（eg:除数为0）

    内核必须处理的异常条件产生的(eg:缺页）

b. 异常又分为故障和陷阱，它们都不使用中断控制器，也不能被屏蔽

c. X86处理处理器中大约有20中异常。Linux内核必须为每种异常提供一个专门的异常处理程序。

2. 中断:也称为异步中断。因此它是由其他硬件设备依照 CPU 时钟信号随机产生，即意味着中断能在指令之间发生。

a. 中断又分为外部可屏蔽中断(INTR)和外部非屏蔽中断（NMI）

    所用I0设备产生的中断请求均引起可屏蔽中断

    硬件故障引起的故障则产生非屏蔽中断。


3. 在CPU执行一个异常处理程序时，就不再响应其他异常和中断请求服务.那么如果此时发生了一个异常，CPU不能去响应它，又不能把它的信息丢失该怎么办呢？

这是就用到了堆栈，把所有的信息压入栈。等当前异常处理后，才从堆栈中取出信息再响应刚才的异常。(当产生多个非屏蔽中断时，CPU的处理方法同上）

2 硬中断： 硬件中断
--------------------

由与系统**相连的外设**(比如网卡、硬盘)自动产生的。主要是用来通知操作系统系统外设状态的变化。比如当网卡收到数据包的时候，就会发出一个中断。我们通常所说的中断指的是硬中断(hardirq)。


3 软中断： 软件中断
--------------------------

为了满足实时系统的要求，中断处理应该是越快越好。

linux为了实现这个特点，当中断发生的时候，硬中断处理那些短时间就可以完成的工作，而将那些处理事件比较长的工作，放到中断之后来完成，也就是软中断(softirq)来完成。

### 硬中断和软中断的区别

```html
首先明确上述定义，硬中断和软中断都是一种中断信号；外设只是产生中断信号的原因。

所以，二者区分的方向就是作用不一样的两种中断信号而已。

硬中断是短时间能完成的工作就发个硬中断信号；而长时间的就先发一个中断，之后用软中断处理。
```

软中断是执行中断指令产生的，而硬中断是由外设引发的。

硬中断的中断号是由中断控制器提供的，软中断的中断号由指令直接指出，无需使用中断控制器。

硬中断是可屏蔽的，软中断不可屏蔽。(处理一种硬中断的时候，屏蔽所有同类型中断)

硬中断处理程序要确保它能快速地完成任务，这样程序执行时才不会等待较长时间，称为上半部。

软中断处理硬中断未完成的工作，是一种推后执行的机制，属于下半部。

#### 软中断指令（调用raise_softirq()来触发软中断）

软中断是一组静态定义的下半部接口，可以在所有处理器上同时执行，即使两个类型相同也可以。

但一个软中断不会抢占另一个软中断，唯一可以抢占软中断的是硬中断。

int是软中断指令。

中断向量表是中断号和中断处理函数地址的对应表。

int n - 触发软中断n。相应的中断处理函数的地址为：中断向量表地址 + 4 * n。

#### 中断嵌套

Linux下硬中断是可以嵌套的，但是没有优先级的概念，也就是说任何一个新的中断都可以打断正在执行的中断，但同种中断

除外。软中断不能嵌套，但相同类型的软中断可以在不同CPU上并行执行。
