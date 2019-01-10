---
layout:     post
title:      技术栈1 - Redis(3) - redis事务
no-post-nav: true
category: stack
tags: [stack]
excerpt: 
---

P56：
#### 基本的redis事务

```html
1、概念：
  1、redis层面：
    在redis中，被multi和exec命令包围的命令会一个接一个执行，知道所有命令都执行结束之后才会处理其他客户端的命令。
    1、先执行multi命令
    2、接到multi命令后，将客户端发送的命令放入到一个队列里面，知道客户端发送exec命令结束
    3、redis不被打断的一个一个的执行队列中的命令；
  2、python层面：
    redis事务在python客户端上是由流水线(pipeline)实现的。
    pipeline()相当于multi，execute()相当于exec命令。

    1、pipeline = conn.pipeline() 开启一个事务；
    2、pipeline.incr 增加命令到队列
    3、pipeline.execute() 执行命令。

2、优点
  1、移除竞争条件
  2、提高性能
    redis每条命令都是客户端和服务器的一次通信，pipeline可以一次通信执行所有的命令。

p76:
2、redis事务

  1、关系型事务 和 redis事务
    关系型事务：
      1、用户向数据库发送begin
      2、执行相互一致(consistent)的写操作和读操作
      3、commit提交修改 或者 rollback回滚操作。
    
    redis事务如上所示。
    问题：redis只有执行exec命令了才会执行，在此之前数据不会发生变化，所以在exec执行之前就无法判断是不是数据一致，因为没有实质操作数据。
    redis命令组合：
      multi 和 exec
      watch对键进行监视； pipe.watch (键应该就是某一次事务操作的标识)
      discard 忽略已经加入队列的某条命令

  2、乐观锁（pessimistic locking） 和 悲观锁（optimistic locking）
    1、redis为什么没实现典型的加锁操作？
      悲观锁，问题是持有锁的客户端执行多久，等待解锁的客户端就要阻塞多久。

      乐观锁是如果其他客户端抢先修改了数据，redis只会通知执行了watch命令的客户端，然后客户端失败重试就可以了，而不必花时间等待。
      （watch观察者模式，客户端被通知）

```

#### 非事务型 流水线

> 不使用事务，但使用流水线，目的是为了减少通信次数

python实现：

  conn.pipeline(True)  # 使用事务

  conn.pipeline(False)  # 不使用事务

#### 其他改善性能的选项

命令： redis-benchmark (-c l -q)

展示一些常用命令在1秒内可以执行的次数。

一般python客户端是所示性能的50%~60%。

![](https://hunzino1.github.io/assets/images/2019/redis/benchmark.png)

![](https://hunzino1.github.io/assets/images/2019/redis/question.png)
