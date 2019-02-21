---
layout:     post
title:      技术栈2 - MySQL(1) - MySQL知识树(初级部分)
category: stack
tags: [stack]
excerpt: 
---

MySQL知识树(初级部分)
====================

该文档涵盖了MySQL初级程序员所需要学习的知识.

阅读完该文档后，您将会了解到MySQL:

* MySQL基本命令.
* MySQL索引, 锁.
* 如何分析慢查询.
* MySQL规范
* 参考书籍

--------------------------------------------------------------------------------

NOTE: 下面是学习计划建议
1. MySQL基本命令(2天)
2. MySQL索引, 锁的概念(1天)
3. 阅读必读书籍(2天)
4. MySQL规范(2天)
5. 分析和解决性能问题(1天)

MySQL基本命令(建议学习时间: 2天)
-------------------------------
### MySQL环境搭建

进入MySQL server

```
mysql -uroot
```

### 常用命令汇总
INFO: 请结合[MySQL必知必会](http://pan.baidu.com/s/1nuoBQ2H)学习下面的章节

点击[这里](http://www.w3school.com.cn/sql/sql_quickref.asp), 查看所有的命令的示例

#### [DDL(Data Definition Language)](https://dev.mysql.com/doc/refman/5.6/en/glossary.html#glos_ddl)
> DDL, data definition language, a set of SQL statements for manipulating the database itself rather than individual table rows.

- **CREATE** TABLE/INDEX/VIEW/DATABASE
- **ADD** INDEX/COLOMN
  * PRIMARY KEY
  * UNIQUE INDEX
- **ALTER** TABLE
- **DROP** TABLE/INDEX
- **TRUNCATE** TABLE

#### [DML(Data Manipulation Language)](https://dev.mysql.com/doc/refman/5.6/en/glossary.html#glos_dml)

> DML, data manipulation language, a set of SQL statements for performing insert, update, and delete or select operations.

- SELECT
  * WHERE(AND/OR/UNION, IN/BETWEEN/LIKE)
  * JOIN(INNER, LEFT, RIGHT, FULL)
  * ORDER BY
  * DISTINCT
  * GROUP BY, HAVING
  * COUNT
  * SUM
  * MIN/MAX
- INSERT
- UPDATE
- DELETE

#### 其他
- NOW
- NULL
- SHOW CREATE TABLE

索引和锁(建议学习时间: 1天)
----------------------------
### 索引
#### 基本命令
INFO: 请结合[MySQL必知必会](http://pan.baidu.com/s/1nuoBQ2H)学习下面的章节

- 创建索引 `CREATE INDEX`
- 唯一性索引 `CREATE UNIQ INDEX`
- 删除索引 `DELETE INDEX`
- 查看索引 `SHOW INDEX FROM xxx`

### 锁的基本概念
INFO: 请结合[高性能MySQL](http://pan.baidu.com/s/1jHoHYr8)第一章学习下面的内容

- 并发控制: 读/写锁
- 事务, ACID
- 隔离级别
- 死锁
- 多版本并发控制(MVCC)

### 锁问题
#### 写锁问题
INFO: 请阅读[高性能MySQL](http://pan.baidu.com/s/1jHoHYr8)的第一章的并发控制部分

无法获取到写锁一般有两种情况:

1. 在事务中执行了更新操作, 获取到了写锁, 但是在事务中迟迟没有进行commit, 如

    在 A 进程中, 对id为1024的用户`user`进行了更新操作, 但是由于某个原因

    ```ruby
    ActiveRecord::Base.transction do
      user = User.find(1024)
      user.name = 'xiaoming'
      user.save

      ## 因为某种原因, 该进程被hang住了, 下面是一个hang住的例子
      sleep(1000)
    end
    ```

    此时在 B 进程中, 对1024的`user`再进行更新操作, 则获取不到写锁

    ```ruby
      user = User.find(1024)
      user.name = 'xiaohong'
      user.save
    ```

2. [死锁](http://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_deadlock)

  请阅读[高性能MySQL](http://pan.baidu.com/s/1jHoHYr8)的第一章的死锁部分

#### 超卖问题
> 超卖的例子: 假设商品的库存有10件, 在某个时间段大量用户同时抢购该商品, 导致商品卖出的数目大于10件, 超出了库存数目.

导致超卖现象的一种伪代码如下:

```ruby
# 接收到用户购买请求
if product.can_buy_count > 0                        # 如果还有库存剩余
  product.can_buy_count = product.can_buy_count - 1 # 将库存数减一
  product.save                                      # 更新商品的库存数
else
  # 告诉用该商品已经卖光
end
```

出现超卖在原因: 在`判断是否还有库存剩余`和`将库存数减一` 这两个操作之间, 存在了其他的
进程修改了 库存数, 即:

```ruby
# 接收到用户购买请求
if product.can_buy_count > 0                        # 如果还有库存剩余
  ##  此时其他的进程改了 can_buy_count 的值
  product.can_buy_count = product.can_buy_count - 1 # 将库存数减一
  product.save                                      # 更新商品的库存数
else
  # 告诉用该商品已经卖光
end
```

解决上述的方法为: 添加MySQL的行间锁

参考书籍(建议学习时间: 2天)
--------
### 必读部分
- [MySQL必知必会](http://pan.baidu.com/s/1nuoBQ2H) 1-21章, 26章, 30章, 附录C, 附录D, 附录E
- [W3C MySQL](http://www.w3school.com.cn/sql/) SQL 基础教程; SQL 高级教程
- [高性能MySQL](http://pan.baidu.com/s/1jHoHYr8) 第1章

### 选读部分
- [MySQL技术内幕 InnoDB存储引擎 第2版](http://pan.baidu.com/s/1pJSnnQV)
- [高性能MySQL](http://pan.baidu.com/s/1pJSnnQV)

MySQL规范(建议学习时间: 2天)
--------------------------
1. [表结构设计规范](https://hunzino1.github.io/stack/2019/01/22/mysql_table_design)
2. [运维规范](https://hunzino1.github.io/stack/2019/01/22/mysql_specification)

分析和解决慢查询问题(建议学习时间: 1天)
---------------------------------------
初级部分主要分析的是由于 `未添加合适的索引` 导致的慢查询问题

感兴趣的同学可以阅读[高性能MySQL](http://pan.baidu.com/s/1jHoHYr8)第五章的内容

### SQL语句优化
可考虑的优化思路为:

1. 将该SQL移至从库
2. 使用 [EXPLAIN](http://dev.mysql.com/doc/refman/5.7/en/explain.html#idm140230885036768)

    通过EXPLAIN可以看到[执行计划](http://www.cnitblog.com/aliyiyi08/archive/2008/09/09/48878.html)

    ```
    EXPLAIN SELECT SQL_NO_CACHE `deals`.* FROM `deals`  WHERE ( deals.bg_tag_id > 0 ) AND `deals`.`id` = 1
    +----+-------------+-------+-------+-----------------------------------------------+---------+---------+-------+------+-------+
    | id | select_type | table | type  | possible_keys                                 | key     | key_len | ref   | rows | Extra |
    +----+-------------+-------+-------+-----------------------------------------------+---------+---------+-------+------+-------+
    |  1 | SIMPLE      | deals | const | PRIMARY,idx_bg_tag_pub_beg,idx_bg_tag_pub_end | PRIMARY | 4       | const |    1 |       |
    +----+-------------+-------+-------+-----------------------------------------------+---------+---------+-------+------+-------+
    1 row in set (0.02 sec)
    ```

    其中最重要的几列有:

        - `key`列 显示MySQL实际决定使用的键（索引）。要想强制MySQL使用或忽视possible_keys列中的索引，在查询中使用FORCE INDEX、USE INDEX或者IGNORE INDEX。
        - `rows`列 显示MySQL认为它执行查询时必须检查的行数。
        - `extra`列, 查看 MySQL解决查询的详细信息, 如果出现 `Using filesort` 或 `Using temporary`, 则需要优化查询

3. 根据第二步的执行计划, 可考虑添加相关索引.
4. 如果数据量过大, 添加索引无法解决问题, 则可以考虑分表.
5. 出现`like查询`或者`全文索引`等查询, 需要考虑用其他的方式来解决该慢查询.
