---
layout:     post
title:      技术栈1 - Redis(4) - redis核心概念
category: stack
tags: [stack]
excerpt: 已坚持多时,却失在最后一刻.
---

> 一辈子有多少的来不及和差一点！

redis核心概念
=======

该文档涵盖了 redis 的核心的一些概念, 包括redis服务的分布式锁、计数信号量、任务队列、有序索引.

阅读完该文档后，您将会了解到:

* 分布式锁
* 计数信号量
* 任务队列
* 有序索引

这篇博客若有错误之处还望指正。

--------------------------------------------------------------------------------

分布式锁
----------

### 1. watch事务的弊端：

  在高负载、高并发情况下，由于watch是乐观锁，在事务执行失败时会进行不断的重试，从而导致效率下降。
  此时如果使用一个排他性锁，会大大介绍重试的情况。

### 2. 锁的基本功能（简易锁）

  1. 定义锁（lock）
  2. 加锁  （locking）
  3. 锁释放（release lock）
  4. 锁超时（timeout）

### 3. 简易锁可能存在的异常：

  - 持有锁的线程因操作时间过长而导致锁自动释放，而进程并不知道已经释放或者错误的释放了其他获取锁的进程；
  - 持有锁的进程崩溃了，而其他线程还在等待浪费资源；
  - 上述持有锁的进程释放后，多个线程同时获取了锁；
  - 1/3同时发生，每个获得锁的进程都认为自己是惟一的；

### 4. 简易锁构建及使用

  ```html
    命令： setnx （string： SETNX key value）
           setnx = set if not exists(如果不存在，则 SET)。

           redis> EXISTS job                # job 不存在
           (integer) 0

           redis> SETNX job "programmer"    # job 设置成功
           (integer) 1

           redis> SETNX job "code-farmer"   # 尝试覆盖 job ，失败
           (integer) 0

           redis> GET job                   # 没有被覆盖
           "programmer"

  ```

1. 基本加锁功能的实现

  ```python
    ## 获取锁
    def acquire_lock(conn, lockname, acquire_timeout=10):
        identifier = str(uuid.uuid4())        # 128uuid随机码

        end = time.time() + acquire_timeout
        while time.time() < end:     #超时时间范围内重试
            if conn.setnx('lock:' + lockname, identifier):  #注意key是唯一的，随机码是value，尝试获取锁,key存在会返回false
                return identifier

            time.sleep(.001)

    return False

    ## 释放锁
    def release_lock(conn, lockname, identifier):
        pipe = conn.pipeline(True)
        lockname = 'lock:' + lockname

        while True:
           try:
               pipe.watch(lockname)                  # 引入事务
               if pipe.get(lockname) == identifier:  # 随机码的作用，检查线程是否还在持有锁
                   pipe.multi()                      #
                   pipe.delete(lockname)             # 释放锁
                   pipe.execute()                    #
                   return True                       #

               pipe.unwatch()
               break

          except redis.exceptions.WatchError:       # 若其他客户端修改了锁,重试
              pass                                  #

    return False
  ```

以上实现了排它锁的基本功能，而竞争的产生主要取决于锁的粒度大小，粒度越小，竞争越少。

2. 锁过期或持有锁进程崩溃

  上面基本锁的实现，在进程崩溃的时候会一直持有锁，导致其他线程只能无法执行；
    所以对锁持有进程要加一个超时功能。

```python
def acquire_lock_with_timeout(
    conn, lockname, acquire_timeout=10, lock_timeout=10):

    identifier = str(uuid.uuid4())
    lockname = 'lock:' + lockname
    lock_timeout = int(math.ceil(lock_timeout)) # 超时

    end = time.time() + acquire_timeout
    while time.time() < end:
        if conn.setnx(lockname, identifier):            # 获取锁，加上超时
            conn.expire(lockname, lock_timeout)         #
            return identifier
        elif conn.ttl(lockname) < 0:                    # 检查锁的过期时间，没有则添加上
            conn.expire(lockname, lock_timeout)

        time.sleep(.001)

    return False

```

计数信号量(counting semaphore)
-------------------------------
> 虽不如上述的一般锁使用频繁，但对于让多个客户端同时访问信息时，计数信号量就比较适用了。

### 计数信号量概念
用途：

计数信号量可以限制一个资源最多能同时被多少个线程来访问使用。

与其他锁的区别

  - 计数信号量
    客户端获取锁失败时，会立刻返回失败结果；
  - 其他锁
    失败的时候会等待。

### 构建基本的计数信号量
> 构造技术信号量的方法有多种，但是若不考虑信号量超时或者持有信号量进程崩溃的情况，构建没有太大的实用价值。

实现技术信号量的两种方式：

1. 如分布式锁一样，引入expire超时
2. 使用有序集合(zset)， 将持有计数信号量的所有持有者存入一个有序结构进行管理。

```python
# 有序集合实现步骤：
# 1、检查有序集合，清理掉过期元素
# 2、将客户端A请求存入有序集合，key为128为标识符，分数是时间戳
# 3、计算A在有序集合的排名，若小于计数信号量总数则成功获得信号量，反之失败
def acquire_semaphore(conn, semname, limit, timeout=10):
    identifier = str(uuid.uuid4())
    now = time.time()

    pipeline = conn.pipeline(True)
    pipeline.zremrangebyscore(semname, '-inf', now - timeout)  # 清楚过期信号量持有者
    pipeline.zadd(semname, identifier, now)                    # 加入有序集合
    pipeline.zrank(semname, identifier)                        # 排序
    if pipeline.execute()[-1] < limit:                         # 获得持有信号量
        return identifier

    conn.zrem(semname, identifier)                             #获取失败，删除之前添加
    return None

# 释放信号量直接删除即可
def release_semaphore(conn, semname, identifier):
    return conn.zrem(semname, identifier)  

```

3. 公平信号量

#### 上述实现的问题
> 不同机器本地系统时间不一定完全一致，机器A、B，若机器A比B晚了10毫秒，那么A可以在B获取信号量之后10ms内竞争获胜取得锁。

#### 解决方案

1. 通过某种方案使得各个机器的时间误差小于1ms；
2. 引入一个计数器，每获取一个信号量，机器信息 + 计数器自增存入计数器，不再以时间作为分数，而是计数器的值作为分值。

一句话，不再用不公平的信号量时间戳，引入一个公平的计数器即可。

释放的时候，要删除计数器对应信息。

任务队列
----------
> 任务队列和sidekiq如出一辙，结合理解。

有序索引
----------

