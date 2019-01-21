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

分布式锁(p115)
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

- 1. 基本加锁功能的实现

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

- 2. 锁过期或持有锁进程崩溃

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

计数信号量(counting semaphore)p126
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

任务队列(p133)
----------
> 任务队列和sidekiq如出一辙，结合理解。

### 1. 定义
在处理一些命令请求时，有些操作完成是需要一段时间的。通过将执行任务的参数信息放入任务队列中，之后可以对队列进行处理，

像这样任务可以容忍推迟执行的，将工作交给任务处理器的做法称为任务队列（task queue）。

任务队列有两种形式：
1. 即时执行： 按照放入队列的顺序立刻执行；
2. 定时执行： 安排在未来的某个时间点执行；

### 2. 理解（结合sidekiq）

sidekiq其实就是基于redis实现的异步处理工具，所以实现上的原理应该基本一致。
[sidekiq分析](https://hunzino1.github.io/rails_stack/2019/01/11/sidekiq2.html)

```html
1、 即时执行
  将数据放入redis有序队列，找到对应的处理方法处理参数。

2、 定时执行
  将数据放入特定队列，轮训，然后加入到上述实时处理队列。
```

### 3. 实现

已经说是使用队列结构了，要就先进先出，在redis中对应list列表了。

- 实时处理

```html
rpush: RPUSH key value [value ...]
       将一个或多个值 value 插入到列表 key 的表尾(最右边)。
       如果有多个 value 值，那么各个 value 值按从左到右的顺序依次插入到表尾

blpop: BLPOP key [key ...] timeout
       BLPOP 是列表的阻塞式(blocking)弹出原语。
       它是 LPOP 命令的阻塞版本，当给定列表内没有任何元素可供弹出的时候，连接将被 BLPOP 命令阻塞，直到等待超时或发现可弹出元素为止。
       当给定多个 key 参数时，按参数 key 的先后顺序依次检查各个列表，弹出第一个非空列表的头元素。
```

> 此外，还可以有任务优先级命令，也就是弹出顺序，blpop可设置

- 定时执行

使用有序集合zset，按照时间戳作为分值，可以快速将需要处理的定时任务放入实时处理队列中。

有序索引(p162)
----------
> 搜索引擎如何才能根据用户输入的关键字迅速定位一组最可能的搜素结果，这就需要构建一个有序索引，将找到的结果进行排序反馈。

这里并不介绍太多，可以根据[redis5](https://hunzino1.github.io/stack/2019/01/17/stack_redis5.html)中的搜索应用程序中的总结进行理解。

在此大概介绍一个例子。

```html
文档搜索引擎：
1、实现对所有的文档中的每个词语都建立索引。(除去量词和无用词集合如and、or等中的词)
2、根据用户输入的关键词找到候选文档。 
  比如用户输入： C++ 程设 大赛
  因为1已经对每个文档中每个词建立了索引，所以能很快找出包含这三个词或同义词的文档列表。
3、这些文档组合如何选出最符合的10个结果。
  就会根据更新时间、题目信息、地理位置等因素计算出一个分数，然后排序。
  那么很显然，有序集合zset。
```

以上只说说一个简单例子理解一下什么是有序索引。

总结
------

其实上面罗列的其实也应该算是redis应用，只是中间可能接入了数据结构知识，看起来比较有模块化，所以才与下一章节的应用分开表述。
