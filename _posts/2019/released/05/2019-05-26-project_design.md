---
layout:     post
title:      设计一个电商平台积分兑换系统
category: stack
tags: [stack]
excerpt: 一个人有多努力，就会有多幸运。
---

[设计一个电商平台积分兑换系统](https://maimai.cn/article/detail?fid=1252495809&efid=0fjwMCDG17Y5XvRkRRwE4g)
=======================================

类似的这类系统设计题目很多，比如：

请你设计一个秒杀系统

请你设计一个支撑百万用户的IM消息系统

请你设计一个微信红包系统

请你设计一个电商平台积分兑换系统

这些题目本身都是开放式命题，没有固定答案。遇到这种问题，一定不要慌，关键是在现场要思路清楚，有理有据，慢慢分析。

-------------------------------------

1 设计思路
------------------------------------

对这类开放式面试题的一个回答思路，首先得从业务上来考虑这个系统应该有哪些业务组成部分，如何实现业务流程？

其次就是你得考虑面临对应的业务场景的时候，这个系统会有哪些技术挑战，各个环节可能会有哪些技术问题？

然后应该针对这些技术挑战和技术问题，现场给出一些你的思路。只要给出大致的思路就可以，比如应该往哪个方向去解决，应该引入哪些机制。

```html
说实话，大部分人是没实际做过这类系统的。比如让你设计一个秒杀系统，如何设计？

试问，国内有多少人真的做过秒杀系统？其实面试官只不过是通过这个问题考察你的技术面、技术功底，
对各种常规技术方案的积累，以及现场分析业务，分析技术问题，进而基于你过去的技术积累，给出合理解决方案的一个能力。
```

总结起来就是：

1. 业务逻辑设计 --    表结构设计
2. 业务场景     --    事务等
3. 技术挑战     --    中间件，重试机制，幂等算法等

下面是各个面试案例的具体思路：

2 设计一个电商平台积分兑换系统
-------------------------------------

[传送门](https://maimai.cn/article/detail?fid=1252495809&efid=0fjwMCDG17Y5XvRkRRwE4g)

### 2.1 业务逻辑设计(表结构设计)

如下，业务逻辑设计和表结构分析并不是太难的事，这一步的难点是逻辑思维是否缜密，

> 比如是否考虑到查看物流信息等；

| 表名      |表结构|
| ----      | --   |
| 积分表    | 略     |
| 积分兑换表|  略    |
| 发货申请表|  略    |

![pro_design_data](https://hunzino1.github.io/assets/images/2019/interview/pro_design_data.png)

### 2.2 业务场景设计（事务）

第一步只是给出了基本的数据库表定义；

而具体的业务逻辑实现需要考虑到数据增删改查问题了；

所以要考虑是否需要用到事务？

> 扣减积分、新增积分兑换记录、新增发货申请单，这三个步骤必须是要么一起完成，要么一起失败的。
> 
> 也就是说，这三个步骤必须是在一个事务里的。

而如上图，积分和物流是两个服务，相互独立，怎么能保证事务？

其中不太好的一个思路是分布式事务。

但是其实有根间的方案：

> 在事务的最后一步调用仓储服务的接口，如果接口调用成功就提交事务；调用失败则抛异常让事务回滚即可。

![pro_design_transaction](https://hunzino1.github.io/assets/images/2019/interview/pro_design_transaction.png)

### 2.3 技术挑战

问题： 其实用户操作积分兑换后，其实不必关心仓储服务是否成功，即不必一直等待服务的响应。

解决方案： 增加中间件，给仓储服务发一条增加发货申请表记录的消息即可；

添加成功后仓储服务会发一条回执；

那么，万一接受失败了呢？

解决方案：重试机制保证可靠性，如果在时间阈内没有收到反馈，重发；

那么，万一重复消费了呢？

解决方案： 幂等性机制，将积分兑换表记录的id传给储服务，已存在的记录直接返回(唯一性索引)；

![pro_design_score](https://hunzino1.github.io/assets/images/2019/interview/pro_design_score.png)

### 2.4 总结

以上就是一个简单的设计思路，这里只针对面试，并不是真正的实现一个可执行系统，只是一个思路的提供；

下面再总结几个常见的系统设计问题，并不会想上述这般详细了，领会思路即可；

3 请你设计一个微信红包系统（和秒杀一样，一个意思）
----------------------------------------------

### 3.1 一脸懵逼

> 虽然已经有上面的设计经验了，但是刚拿到这个题目时还是一脸懵逼，表结构除了红包表其他什么都想不起来...

正常是因为没有经验，也简单，站在产品的角度即可，从零开始；

首先红包类型： 群红包 和 个人红包

```html
  个人红包很简单，
      两个人是好友
      发送发先发红包，接收方收到消息
      接收方收到红包
  群红包有些复杂。
```

群红包包括红包的个数和群成员的个数，然后还有抢红包的高并发，红包入账；

总结起来就是微信红包的业务包含包、发、抢、拆、查询发送红包和收红包数量，其中最关键的步骤是发红包和抢红包。

以上就是思路，按照产品的设计思路，一步步就有个业务的框架和技术的想法；

### 3.2 业务设计

#### 3.2.1 关键设计

mysql + redis的存储方式

- 通过cache缓存抵挡大部分请求（是否能拆红包等）
- DB使用CAS操作更新红包计数记录 (compare and set)

#### 3.2.2 业务逻辑

**1 新建红包**

  在DB、cache各新增一条记录

**2 抢红包**

    请求访问cache，剩余红包个数大于0则可拆开红包

**3 拆红包**

1. 请求访问cache，剩余红包个数大于0则继续，同时获取可抢红包数与金额
2. 计算金额（从1分到剩余平均值2倍之间随机数，如果不是最后一个红包，剩余金额预留最少1分给cas更新失败，最后一位拿红包的人）
3. cas更新数据库（更新红包计数表记录【剩余红包个数、剩余红包金额】、插入领取记录）
  1. 更新失败，重复以上操作，直到更新成功或已领取完
  2. 更新成功，更新缓存

```java
while (hadHongBao()) {
    //剩余红包个数
    def remainCount = getRemainCount() 
    //实时计算获取红包金额
    def getAmount = calculateAmount() 
    def result = sql.excute("update '红包计算表' set balance=${total-getAmount}, remainCount=${remainCount-1} where remainCount=${remainCount} and id=${id}")
    // 更新失败既继续执行循环，直到更新成功或已领取完，达到CAS效果
    if (result > 0) {
        // 更新成功，执行更新缓存等后续操作
        // ......
        break
    }
}
```

### 3.3 DB、cache主要数据结构

- DB：
  1. 红包计数表【主要字段：红包总金额、红包总个数、剩余红包个数、剩余红包金额】
  2. 红包领取表
- cache：
  1. 红包计数记录【主要字段：剩余红包个数、剩余红包金额】
  2. 红包领取表

### 3.4 另一种思路

预分配金额的实现方式：（新建红包时已分配好每一位抢到红包的金额），减少实时计算及CAS操作的性能损耗，

但是预分配金额还需要额外存储空间（更重要的可能是现在的实现方式已满足需求），

后来新浪微博Tim总提出预分配方式不占用额外存储空间的方式，详细请看：

<a href="http://timyang.net/architecture/wechat-red-packet/">微信红包金额分配的算法</a>

实现原理：

1. 通过保存random seed达到预分配金额效果，既无需记录剩余红包金额，只需记录红包剩余数
2. 抢红包时，针对红包剩余数进行DESC原子操作，当红包剩余数为0既抢完

### 3.5 第一种方案的总结

```html
1 微信的金额什么时候算？

答：微信金额是拆的时候实时算出来，不是预先分配的，采用的是纯内存计算，不需要预算空间存储。
采取实时计算金额的考虑：预算需要占存储，实时效率很高，预算才效率低。

2 实时性：为什么明明抢到红包，点开后发现没有？

答：2014年的红包一点开就知道金额，分两次操作，先抢到金额，然后再转账。

2015年的红包的拆和抢是分离的，需要点两次，因此会出现抢到红包了，但点开后告知红包已经被领完的状况。
进入到第一个页面不代表抢到，只表示当时红包还有。

3 分配：红包里的金额怎么算？为什么出现各个红包金额相差很大？

答：随机，额度在0.01和剩余平均值2之间。

例如：发100块钱，总共10个红包，那么平均值是10块钱一个，那么发出来的红包的额度在0.01元～20元之间波动。
当前面3个红包总共被领了40块钱时，剩下60块钱，总共7个红包，那么这7个红包的额度在：0.01～（60/72）=17.14之间。
注意：这里的算法是每被抢一个后，剩下的会再次执行上面的这样的算法（Tim老师也觉得上述算法太复杂，不知基于什么样的考虑）。
这样算下去，会超过最开始的全部金额，因此到了最后面如果不够这么算，那么会采取如下算法：保证剩余用户能拿到最低1分钱即可。

如果前面的人手气不好，那么后面的余额越多，红包额度也就越多，因此实际概率一样的。

4 红包的设计

答：微信从财付通拉取金额数据郭莱，生成个数/红包类型/金额放到redis集群里，

app端将红包ID的请求放入请求队列中，如果发现超过红包的个数，直接返回。

根据红包的裸祭处理成功得到令牌请求，则由财付通进行一致性调用，通过像比特币一样，两边保存交易记录，

交易后交给第三方服务审计，如果交易过程中出现不一致就强制回归。

5 高并发处理：红包如何计算被抢完？

答：cache会抵抗无效请求，将无效的请求过滤掉，实际进入到后台的量不大。

cache记录红包个数，原子操作进行个数递减，到0表示被抢光。财付通按照20万笔每秒入账准备，但实际还不到8万每秒。

6 通如何保持8w每秒的写入？

答：多主sharding，水平扩展机器。

7 据容量多少？

答：一个红包只占一条记录，有效期只有几天，因此不需要太多空间。

9 询红包分配，压力大不？

答：抢到红包的人数和红包都在一条cache记录上，没有太大的查询压力。

10 一个红包一个队列？

答：没有队列，一个红包一条数据，数据上有一个计数器字段。

11 有没有从数据上证明每个红包的概率是不是均等？

答：不是绝对均等，就是一个简单的拍脑袋算法。

12 拍脑袋算法，会不会出现两个最佳？

答：会出现金额一样的，但是手气最佳只有一个，先抢到的那个最佳。

13 每领一个红包就更新数据么？

答：每抢到一个红包，就cas更新剩余金额和红包个数。

14 红包如何入库入账？

数据库会累加已经领取的个数与金额，插入一条领取记录。入账则是后台异步操作。

15 入帐出错怎么办？比如红包个数没了，但余额还有？

答：最后会有一个take all操作。另外还有一个对账来保障。
```