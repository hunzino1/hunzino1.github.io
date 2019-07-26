---
layout:     post
title:      技术栈2 - MySQL(8) - 总结4
no-post-nav: true
category: stack
tags: [stack]
excerpt: 
---

#### 一、InnoDB在什么情况下主键不是按照正序排列的呢？

索引如下:

```
CREATE TABLE `pay_account_logs` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `account_id` int(11) NOT NULL DEFAULT '0' ,
  `b_amount` int(11) NOT NULL DEFAULT '0' ,
  `r_amount` int(11) NOT NULL DEFAULT '0' ,
  `amount` int(11) NOT NULL DEFAULT '0' ,
  `log_type` int(11) NOT NULL DEFAULT '0' ,
  `created_at` datetime NOT NULL,
  `updated_at` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `idx_created_at` (`created_at`,`id`),
  KEY `idx_account_type` (`account_id`,`log_type`,`id`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8;
```



**三个column 的索引**

```
mysql> explain SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408 AND (created_at >= '2015-08-23 00:00:00');
+----+-------------+------------------+------+---------------------------------+------------------+---------+-------+------+-------------+
| id | select_type | table            | type | possible_keys                   | key              | key_len | ref   | rows | Extra       |
+----+-------------+------------------+------+---------------------------------+------------------+---------+-------+------+-------------+
|  1 | SIMPLE      | pay_account_logs | ref  | idx_created_at,idx_account_type | idx_account_type | 4       | const |    4 | Using where |
+----+-------------+------------------+------+---------------------------------+------------------+---------+-------+------+-------------+
1 row in set (0.01 sec)


mysql> SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408;
+---------+------------+----------------+---------------+----------+----------+---------------------+---------------------+
| id      | account_id | b_amount | r_amount | amount   | log_type | created_at          | updated_at          |
+---------+------------+----------------+---------------+----------+----------+---------------------+---------------------+
| 3325459 |     157408 |              0 |             0 |  -210000 |        1 | 2015-08-23 13:18:43 | 2015-08-23 13:18:43 |
| 3325445 |     157408 |              0 |             0 | -1000000 |        2 | 2015-08-23 13:14:44 | 2015-08-23 13:14:44 |
| 3325443 |     157408 |        1000000 |             0 |  1000000 |        4 | 2015-08-23 13:13:46 | 2015-08-23 13:13:46 |
| 3325455 |     157408 |         210000 |             0 |   210000 |        4 | 2015-08-23 13:18:17 | 2015-08-23 13:18:17 |
+---------+------------+----------------+---------------+----------+----------+---------------------+---------------------+

结果并不是按照id正序排列的
```

```
explain SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408 order by id desc;
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-----------------------------+
| id | select_type | table            | type | possible_keys              | key                        | key_len | ref   | rows | Extra                       |
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-----------------------------+
|  1 | SIMPLE      | pay_account_logs | ref  | idx_of_account_id_log_type | idx_of_account_id_log_type | 4       | const |    4 | Using where; Using filesort |
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-----------------------------+
1 row in set (0.01 sec)
```

```
explain SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408 order by log_type desc, id desc;
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-------------+
| id | select_type | table            | type | possible_keys              | key                        | key_len | ref   | rows | Extra       |
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-------------+
|  1 | SIMPLE      | pay_account_logs | ref  | idx_of_account_id_log_type | idx_of_account_id_log_type | 4       | const |    4 | Using where |
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-------------+
1 row in set (0.01 sec)
```

```
explain SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408 order by log_type asc, id desc;
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-----------------------------+
| id | select_type | table            | type | possible_keys              | key                        | key_len | ref   | rows | Extra                       |
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-----------------------------+
|  1 | SIMPLE      | pay_account_logs | ref  | idx_of_account_id_log_type | idx_of_account_id_log_type | 4       | const |    4 | Using where; Using filesort |
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-----------------------------+
1 row in set (0.01 sec)
```


```
explain SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408 order by log_type desc;
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-------------+
| id | select_type | table            | type | possible_keys              | key                        | key_len | ref   | rows | Extra       |
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-------------+
|  1 | SIMPLE      | pay_account_logs | ref  | idx_of_account_id_log_type | idx_of_account_id_log_type | 4       | const |    4 | Using where |
+----+-------------+------------------+------+----------------------------+----------------------------+---------+-------+------+-------------+
1 row in set (0.01 sec)
```

数据按照 `account_id`,`log_type`,`id` 升序排序


**两个column的情况**

如果索引是如下方式创建

```
mysql> alter table pay_account_logs add index `idx_of_account_id` (`account_id`, `id`);

mysql> SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408;
+---------+------------+----------------+---------------+----------+----------+---------------------+---------------------+
| id      | account_id | b_amount | r_amount | amount   | log_type | created_at          | updated_at          |
+---------+------------+----------------+---------------+----------+----------+---------------------+---------------------+
| 3325443 |     157408 |        1000000 |             0 |  1000000 |        4 | 2015-08-23 13:13:46 | 2015-08-23 13:13:46 |
| 3325445 |     157408 |              0 |             0 | -1000000 |        2 | 2015-08-23 13:14:44 | 2015-08-23 13:14:44 |
| 3325455 |     157408 |         210000 |             0 |   210000 |        4 | 2015-08-23 13:18:17 | 2015-08-23 13:18:17 |
| 3325459 |     157408 |              0 |             0 |  -210000 |        1 | 2015-08-23 13:18:43 | 2015-08-23 13:18:43 |
+---------+------------+----------------+---------------+----------+----------+---------------------+---------------------+
4 rows in set (0.00 sec)

mysql> explain SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408;
+----+-------------+------------------+------+-------------------+-------------------+---------+-------+------+-------+
| id | select_type | table            | type | possible_keys     | key               | key_len | ref   | rows | Extra |
+----+-------------+------------------+------+-------------------+-------------------+---------+-------+------+-------+
|  1 | SIMPLE      | pay_account_logs | ref  | idx_of_account_id | idx_of_account_id | 4       | const |    4 |       |
+----+-------------+------------------+------+-------------------+-------------------+---------+-------+------+-------+
1 row in set (0.01 sec)
以上是按照id正序排列的
```

```
EXPLAIN SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408 ORDER BY id desc;

+----+-------------+------------------+------+-------------------+-------------------+---------+-------+------+-------------+
| id | select_type | table            | type | possible_keys     | key               | key_len | ref   | rows | Extra       |
+----+-------------+------------------+------+-------------------+-------------------+---------+-------+------+-------------+
|  1 | SIMPLE      | pay_account_logs | ref  | idx_of_account_id | idx_of_account_id | 4       | const |    4 | Using where |
+----+-------------+------------------+------+-------------------+-------------------+---------+-------+------+-------------+
1 row in set (9.51 sec)

以上情况并没有出现 Using filesort 的情况
```

数据按照`account_id`,`id` 升序排列


**在没有索引的情况**

```
mysql> explain SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408;
+----+-------------+------------------+------+---------------+------+---------+------+---------+-------------+
| id | select_type | table            | type | possible_keys | key  | key_len | ref  | rows    | Extra       |
+----+-------------+------------------+------+---------------+------+---------+------+---------+-------------+
|  1 | SIMPLE      | pay_account_logs | ALL  | NULL          | NULL | NULL    | NULL | 1684032 | Using where |
+----+-------------+------------------+------+---------------+------+---------+------+---------+-------------+
1 row in set (0.00 sec)
```


```
mysql> SELECT SQL_NO_CACHE `pay_account_logs`.* FROM `pay_account_logs` WHERE `pay_account_logs`.`account_id` = 157408;
+---------+------------+----------------+---------------+----------+----------+---------------------+---------------------+
| id      | account_id | balance_amount | refund_amount | amount   | log_type | created_at          | updated_at          |
+---------+------------+----------------+---------------+----------+----------+---------------------+---------------------+
| 3325443 |     157408 |        1000000 |             0 |  1000000 |        4 | 2015-08-23 13:13:46 | 2015-08-23 13:13:46 |
| 3325445 |     157408 |              0 |             0 | -1000000 |        2 | 2015-08-23 13:14:44 | 2015-08-23 13:14:44 |
| 3325455 |     157408 |         210000 |             0 |   210000 |        4 | 2015-08-23 13:18:17 | 2015-08-23 13:18:17 |
| 3325459 |     157408 |              0 |             0 |  -210000 |        1 | 2015-08-23 13:18:43 | 2015-08-23 13:18:43 |
+---------+------------+----------------+---------------+----------+----------+---------------------+---------------------+
4 rows in set (2.47 sec)
```

数据按照主键id升序排列。


**联合索引，是依次按照索引顺序，正序排列的**



#### 二、MySQL 关键字，以及关键字带来的坑


```
mysql> desc admin_authorities;
ERROR 2013 (HY000): Lost connection to MySQL server during query
mysql> desc admin_authorities;
ERROR 2006 (HY000): MySQL server has gone away
No connection. Trying to reconnect...
Connection id:    80371
Current database: tao800

+------------+--------------+------+-----+---------+----------------+
| Field      | Type         | Null | Key | Default | Extra          |
+------------+--------------+------+-----+---------+----------------+
| id         | int(11)      | NO   | PRI | NULL    | auto_increment |
| key        | varchar(100) | NO   | UNI |         |                |
| desc       | varchar(100) | NO   |     |         |                |
| label      | varchar(100) | NO   |     |         |                |
| group      | varchar(100) | NO   |     |         |                |
| created_at | datetime     | NO   |     | NULL    |                |
| updated_at | datetime     | NO   |     | NULL    |                |
+------------+--------------+------+-----+---------+----------------+
7 rows in set (12.89 sec)


**where条件之后**

mysql> select * from admin_authorities where key = 'manage_roles'\G;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'key = 'manage_roles'' at line 1
ERROR:
No query specified

mysql> select * from admin_authorities where `key` = 'manage_roles'\G;
*************************** 1. row ***************************
        id: 2
       key: manage_roles
      desc: 权限管理
     label: 权限管理
     group: 权限管理
created_at: 2014-10-28 11:12:02
updated_at: 2014-10-28 11:12:02
1 row in set (0.01 sec)

ERROR:
No query specified

mysql> select * from admin_authorities where admin_authorities.key = 'manage_roles'\G;
*************************** 1. row ***************************
        id: 2
       key: manage_roles
      desc: 权限管理
     label: 权限管理
     group: 权限管理
created_at: 2014-10-28 11:12:02
updated_at: 2014-10-28 11:12:02
1 row in set (0.40 sec)

ERROR:
No query specified


**筛选**

mysql> select desc from admin_authorities where id = 2\G;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'desc from admin_authorities where id = 2' at line 1
ERROR:
No query specified

mysql> select `desc` from admin_authorities where id = 2\G;
*************************** 1. row ***************************
desc: 权限管理
1 row in set (0.00 sec)

ERROR:
No query specified


**分组**


mysql> select count(id), group from admin_authorities group by group;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'group from admin_authorities group by group' at line 1
mysql> select count(id), `group` from admin_authorities group by `group`;
+-----------+-----------------------+
| count(id) | group                 |
+-----------+-----------------------+
|         1 | Cake统计管理          |
|         1 | cpc系统管理           |
|         5 | DealService           |
|         1 | Panda统计管理         |
|         3 | QQ应用                |
|         1 | QQ应用/U站            |
|         3 | 中间页                |
|         6 | 其他功能              |
|         1 | 其它功能              |
|         1 | 双十一活动管理        |
|         1 | 品牌团管理            |
|         1 | 图片管理              |
|        51 | 广告管理              |
|         4 | 广点通                |
|        16 | 库存管理              |
|         8 | 招商平台              |
|         4 | 推广渠道后台          |
|        21 | 无线运营              |
|         7 | 权限管理              |
|         1 | 签到广告              |
|        68 | 聚招商平台            |
|        39 | 运营数据              |
|       115 | 运营编辑              |
+-----------+-----------------------+
23 rows in set (0.09 sec)


```

**以上column key， desc，label，group 均为关键字**

**关键字在查询，排序，分组等SQL语句中都会有异常**

**如果记不住那么多关键字，使用ad_key，ad_desc， ad_label，ad_group 这种自定义前缀的方式命名column**


[MySQL中的关键字](http://www.cnblogs.com/jiangshan5xian/archive/2013/03/16/2963496.html)

#### 三、性能查询优化三驾马车

**查询优化，索引优化，库表结构优化**


#### 四、查询生命周期

从客户端，到服务器，然后在服务器上进行解析，生成执行计划，执行，并返回结果给客户端。其中**执行**可以认为是整个生命周期中最重要的阶段，这其中包括了大量为了检索数据到存储引擎的调用以及调用后的数据处理，包括排序，分组等。

在完成这些任务的时候，查询需要在不同的地方花费时间，包括网络，CPU计算，生成统计信息和执行计划，锁等待（互斥操作）等操作，由其是向底层引擎检索数据的调用操作，这些调用需要在内存操作，CPU操作和内存不足时导致的I/O操作上的消耗时间。

#### 五、是否向数据库请求了不需要的数据

有些查询会请求超过实际需要的数据，然后这些多余的数据会被应用程序丢弃。这会给MySQL服务器带来额外的负担，并增加网络开销，另外这也会消耗应用服务器CPU和内存资源。

所以如非必要，避免使用
** SELECT * FROM xxxx **

每次看到 `SELECT *`的时候都需要用怀疑的眼光审视，是不是真的需要返回全部的列？很可能不是必需的。取出全部列，会让优化器无法完成索引覆盖扫描这类优化。还会为服务器带来额外的I/O，内存和CPU的消耗。


** MySQL 会只返回需要的数据，实际上MySQL却是先返回全部结果集再进行计算 。MySQL会查询出全部的结果集，客户端的应用程序会接受全部的结果集数据，然后抛弃其中大部分数据，那么最有效的解决方式就是加上 limit **


#### 六、多表关联时返回全部列

```

SELECT *
FROM happy_shops
INNER JOIN happy_for_ni on tb_shops.id = happy_for_ni.tb_shop_id
WHERE happy_shops.wangwang = '王二傻'\G;

```

以上查询会返回2个表的全部数据。


#### 七、索引让MySQL 以最高效，扫描行数最少的方式找到需要的记录


#### 八、尽量让MySQL 做最少的逻辑事情，如果SQL太长，可以拆分成多个简单的SQL

MySQL从设计上让连接和断开连接都很轻量级。在返回一个小的查询结果方面很高效。

**查询更多的表，意味着要锁住更多的数据，占满整个事务日志，耗尽系统资源，阻塞很多小的但是重要的查询。通常来说，小事务更加高效。大事务，大语句，不利于高并发。**

**并且使用到简单的查询，能更容易使用到MySQL 的缓存。**


#### 九、SELECT count(id) FROM XXXX 消耗很长的时间

InnoDB因为其MVCC的架构，并不能维护一个数据表的行数的精确统计信息。

[MVCC](http://baike.baidu.com/link?url=QYP70ksDw_mCUxI6TIKulXt3GQbaTgkth3ko3IKHeZvpvzzLT0PT3Glgn765MYgjEJwAgqBIdSRES645MNaq0a)

如果想快速的获取count信息，使用 explain 是一个非常不错的方法

```
mysql> select count(id) from guang_deal_outs;
+-----------+
| count(id) |
+-----------+
|  69864861 |
+-----------+
1 row in set (1 min 12.38 sec)




mysql> explain  select count(id) from guang_deal_outs\G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: guang_deal_outs
         type: index
possible_keys: NULL
          key: idx_guang_dlout_stdate
      key_len: 3
          ref: NULL
         rows: 69865175
        Extra: Using index
1 row in set (0.00 sec)

```

#### 十、前缀索引不能排序

[前缀索引](http://www.jianshu.com/p/85a18c1e7d41)
