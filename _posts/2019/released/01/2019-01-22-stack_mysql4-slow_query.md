---
layout:     post
title:      技术栈2 - MySQL(4) - 慢查询
category: stack
tags: [stack]
excerpt: 
---

慢查询
=======

### 如何鉴定MySQL慢查询


**我们将超过指定时间的SQL语句查询称为慢查询**


#### 一、慢查询的体现

+ 慢查询主要体现在 **慢**上，通常意义上来讲，只要返回时间大于 **>1 sec**上的查询都可以称为慢查询。

+ 慢查询会导致CPU，内存消耗过高。数据库服务器压力陡然过大，那么大部分情况来讲，肯定是由某些慢查询导致的。

**查看/设置“慢查询”的时间定义**

```
mysql> show variables like "long%";
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 0.000100 |
+-----------------+----------+
1 row in set (0.00 sec)
```

如上述语句输出，“慢查询”的时间定义为0.0001秒（方便测试，一般设置为1-10秒）。使用下面语句定义“慢查询”时间

```
mysql> set long_query_time=0.0001;
Query OK, 0 rows affected (0.00 sec)
```

**开启“慢查询”记录功能**

```
mysql> show variables like "slow%";
+---------------------+------------------------------------+
| Variable_name       | Value                              |
+---------------------+------------------------------------+
| slow_launch_time    | 2                                  |
| slow_query_log      | OFF                                |
| slow_query_log_file | /opt/mysql/data/localhost-slow.log |
+---------------------+------------------------------------+
3 rows in set (0.00 sec)
```
上述语句查看“慢查询”的配置信息，你可以自定义日志文件的存放，但必须将 slow_query_log 全局变量设置为“ON”状态，执行以下语句

```
mysql> set global slow_query_log=ON;
Query OK, 0 rows affected (0.01 sec)
```
结果：

```
mysql> show variables like "slow%";
+---------------------+------------------------------------+
| Variable_name       | Value                              |
+---------------------+------------------------------------+
| slow_launch_time    | 2                                  |
| slow_query_log      | ON                                 |
| slow_query_log_file | /opt/mysql/data/localhost-slow.log |
+---------------------+------------------------------------+
3 rows in set (0.00 sec)
```

那么哪些条件可以导致慢查询呢？或者说根据何种条件判断，优化慢查询。仅从SQL语句方面阐述慢查询，MySQL系统级别的设置暂不考虑。

#### 二、返回列数太多

+ 返回列数太多

```
EXPLAIN SELECT `tao_search_statistics`.*
FROM `tao_search_statistics`
ORDER BY nature_total DESC LIMIT 10\G;

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tao_search_statistics
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 66493186  <- 返回值太多，坏味道
        Extra: Using filesort <- 避免使用排序
1 row in set (0.01 sec)

```


优化建议： 添加 关于 nature_total 的索引

```
mysql> alter table tao_search_statistics add index `idx_of_nature_total` (`nature_total`, `id`);
Query OK, 0 rows affected (7 min 48.27 sec)
Records: 0  Duplicates: 0  Warnings: 0


EXPLAIN SELECT `tao_search_statistics`.*
FROM `tao_search_statistics`
ORDER BY nature_total DESC LIMIT 10\G;

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tao_search_statistics
         type: index
possible_keys: NULL
          key: idx_of_nature_total
      key_len: 8
          ref: NULL
         rows: 10
        Extra:
1 row in set (0.01 sec)

```



+ 但是某些情况下，返回列数比较多，也不代表是慢查询。


```
SELECT `candidate_deals`.*
FROM `candidate_deals`
WHERE `candidate_deals`.`publish_status` = 4
  AND `candidate_deals`.`status` IN (0,
                                     1,
                                     2,
                                     3,
                                     4)
  AND (checked_time <= '2015-05-15 23:30:39'
       AND update_time <= '2015-05-15 23:30:39')
ORDER BY `candidate_deals`.`id` ASC LIMIT 1000

+----+-------------+-----------------+-------+---------------------------------------+---------+---------+-----+------+-------------+
| id | select_type | table           | type  | possible_keys                         | key     | key_len | ref | rows | Extra       |
+----+-------------+-----------------+-------+---------------------------------------+---------+---------+-----+------+-------------+
| 1  | SIMPLE      | candidate_deals | index | idx_uptime_seneditor,idx_checked_time | PRIMARY | 4       |     | 2000 | Using where |
+----+-------------+-----------------+-------+---------------------------------------+---------+---------+-----+------+-------------+
```

平均查询时间：6 sec

添加一个 ``idx_of_publish_status_status_checked_time` (`publish_status`,`status`,`checked_time`,`id`)` 索引

```
mysql> explain SELECT SQL_NO_CACHE  `candidate_deals`.* FROM `candidate_deals`  WHERE `candidate_deals`.`publish_status` = 4 AND `candidate_deals`.`status` IN (0, 1, 2, 3, 4) AND (checked_time <= '2015-05-15 23:30:39' and update_time <= '2015-05-15 23:30:39')  LIMIT 1000\G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: candidate_deals
         type: range
possible_keys: idx_uptime_seneditor,idx_checked_time,idx_of_publish_status_sanzu_check_status_signup_status,idx_of_publish_status_status_checked_time
          key: idx_of_publish_status_status_checked_time
      key_len: 16
          ref: NULL
         rows: 3782
        Extra: Using where
1 row in set (0.01 sec)
```

平均查询时间：0.4 sec


#### 三、分页条件，过大的offset

```
EXPLAIN SELECT `deals`.*
FROM `deals`
INNER JOIN `deal_infos` ON `deal_infos`.`deal_id` = `deals`.`id`
WHERE
  (deals.end_time > '2015-08-13 14:08:10')
  AND (deals.bg_tag_id > 0)
  AND (deal_infos.taobao_image_url = '')
ORDER BY deals.updated_at DESC LIMIT 100
OFFSET 28900\G;


+----+-------------+------------+--------+------------------------------------------------------------+---------------+---------+-----------------+--------+-----------------------------+
| id | select_type | table      | type   | possible_keys                                              | key           | key_len | ref             | rows   | Extra                       |
+----+-------------+------------+--------+------------------------------------------------------------+---------------+---------+-----------------+--------+-----------------------------+
| 1  | SIMPLE      | deals      | range  | PRIMARY,idx_end_time,idx_bg_tag_pub_beg,idx_bg_tag_pub_end | idx_end_time  | 8       |                 | 219114 | Using where; Using filesort |
+----+-------------+------------+--------+------------------------------------------------------------+---------------+---------+-----------------+--------+-----------------------------+
| 1  | SIMPLE      | deal_infos | eq_ref | deal_id_index                                              | deal_id_index | 4       | tao800.deals.id | 1      | Using where                 |
+----+-------------+------------+--------+------------------------------------------------------------+---------------+---------+-----------------+--------+-----------------------------+
```

平均查询时间：

  Empty set (9.78 sec)


优化建议：

  1、MySQL里对LIMIT OFFSET的处理方式是，取出OFFSET+LIMIT的所有数据，然后去掉OFFSET，返回底部的LIMIT
  使用子查询，使用覆盖索引进行优化。

  2、这种方式在offset很高的情况下，
  如：limit 100000,20，这样系统会查询100020条，然后把前面的100000条都扔掉，这是开销很大的操作，导致慢查询很慢。

  3、使用覆盖索引(convering index) 查询，然后再跟全行做join操作。这样可以直接使用index 得到数据，而不是去查询表，
  	 当找到需要的数据之后，再与全表join获得其他列。


```
EXPLAIN SELECT SQL_NO_CACHE `deals`.*
FROM `deals`
INNER JOIN
(
SELECT deals.id
FROM deals
INNER JOIN `deal_infos` ON `deal_infos`.`deal_id` = `deals`.`id`
WHERE deals.end_time > '2015-08-13 14:08:10'
      AND deals.bg_tag_id > 0
      AND deal_infos.taobao_image_url = ''
LIMIT 100 OFFSET 28900
) AS lim ON lim.id = deals.id
ORDER BY deals.updated_at DESC \G;

*************************** 1. row ***************************
           id: 1
  select_type: PRIMARY
        table: NULL
         type: NULL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: NULL
        Extra: Impossible WHERE noticed after reading const tables
*************************** 2. row ***************************
           id: 2
  select_type: DERIVED
        table: deals
         type: range
possible_keys: PRIMARY,idx_end_time,idx_bg_tag_pub_beg,idx_bg_tag_pub_end
          key: idx_bg_tag_pub_end
      key_len: 4
          ref: NULL
         rows: 425143
        Extra: Using where; Using index
*************************** 3. row ***************************
           id: 2
  select_type: DERIVED
        table: deal_infos
         type: eq_ref
possible_keys: deal_id_index
          key: deal_id_index
      key_len: 4
          ref: tao800.deals.id
         rows: 1
        Extra: Using where
3 rows in set (0.67 sec)

```

平均查询时间： 0.67 sec

[http://zhidao.baidu.com/link?url=4hVpi7DCSX6bEROu33YQD_Gq2IhERaqOl7yyPASjtixIaRbYhCFEteP4KyJX52RI7QxY0mV_6UM2g8p3KeG4BzJx1z94EozoKW-yewhCDju](http://zhidao.baidu.com/link?url=4hVpi7DCSX6bEROu33YQD_Gq2IhERaqOl7yyPASjtixIaRbYhCFEteP4KyJX52RI7QxY0mV_6UM2g8p3KeG4BzJx1z94EozoKW-yewhCDju)

[http://www.jb51.net/article/27504.htm](http://www.jb51.net/article/27504.htm)

[http://www.jb51.net/article/33777.htm](http://www.jb51.net/article/33777.htm)

[http://chinahnzhou.iteye.com/blog/1567537](http://chinahnzhou.iteye.com/blog/1567537)

[http://dataunion.org/14895.html](http://dataunion.org/14895.html)

[http://blog.idaohang123.com/archives/399](http://blog.idaohang123.com/archives/399)

[http://www.jbxue.com/db/22798.html](http://www.jbxue.com/db/22798.html)

[http://www.admin10000.com/document/4797.html](http://www.admin10000.com/document/4797.html)

[http://www.fienda.com/archives/110](http://www.fienda.com/archives/110)

[http://stackoverflow.com/questions/4481388/why-does-mysql-higher-limit-offset-slow-the-query-down](http://stackoverflow.com/questions/4481388/why-does-mysql-higher-limit-offset-slow-the-query-down)


#### 四、驱动表，临时表，排序

```
mysql> EXPLAIN SELECT `deal_daily_sales`.`deal_id`
    -> FROM `deal_daily_sales`
    -> INNER JOIN deals ON deals.id = deal_daily_sales.deal_id
    -> AND deals.bg_tag_id = deal_daily_sales.tag_id
    -> WHERE (status = 1
    ->        AND date = '2015-08-12')
    -> ORDER BY sales DESC LIMIT 300\G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: deals
         type: index
possible_keys: PRIMARY,idx_bg_tag_pub_beg,idx_bg_tag_pub_end
          key: idx_bg_tag_pub_beg
      key_len: 20
          ref: NULL
         rows: 850279 <- 返回数据太多，坏味道
        Extra: Using index; Using temporary; Using filesort
*************************** 2. row ***************************
           id: 1
  select_type: SIMPLE
        table: deal_daily_sales
         type: eq_ref
possible_keys: deal_id_date_tag_idx,tag_id_date
          key: deal_id_date_tag_idx
      key_len: 11
          ref: tao800.deals.id,const,tao800.deals.bg_tag_id
         rows: 1
        Extra: Using where
2 rows in set (0.00 sec)

ERROR:
No query specified

```

平均查询时间  Empty set (3.90 sec)


EXPLAIN 返回的第一列的表，就是驱动表，在驱动表上排序，非常快。但是如果在非驱动表上，排序，就很慢。
默认情况下，记录就是按照顺序排列好的，不需要进行排序。但是上述结果，从`deals` 表中取出一些数据，建立临时表，并且还在临时表上进行排序。
所以就会出现 **Using temporary; Using filesort** 这种情况。


优化建议：

  1、减少返回值rows

  2、指定正确的驱动表

```
mysql> ALTER table `deal_daily_sales` ADD INDEX `idx_of_date_and_status` (`date`, `status`, `sales`, `id`);
Query OK, 0 rows affected (19.45 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> EXPLAIN SELECT `deal_daily_sales`.`deal_id`
    -> FROM `deal_daily_sales`
    -> STRAIGHT_JOIN deals ON deals.id = deal_daily_sales.deal_id
    -> AND deals.bg_tag_id = deal_daily_sales.tag_id
    -> WHERE  date = '2015-08-12' AND status = 1
    -> ORDER BY `deal_daily_sales`.sales DESC LIMIT 300\G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: deal_daily_sales  <-- 正确的驱动表
         type: ref
possible_keys: deal_id_date_tag_idx,tag_id_date,idx_of_date_and_status
          key: idx_of_date_and_status
      key_len: 7
          ref: const,const
         rows: 1
        Extra: Using where
*************************** 2. row ***************************
           id: 1
  select_type: SIMPLE
        table: deals
         type: eq_ref
possible_keys: PRIMARY,idx_bg_tag_pub_beg,idx_bg_tag_pub_end
          key: PRIMARY
      key_len: 4
          ref: tao800.deal_daily_sales.deal_id
         rows: 1
        Extra: Using where
2 rows in set (0.01 sec)

ERROR:
No query specified

```

平均查询时间：0.03 sec






#### 五、不合适的group by 分组条件

大表上的group by

```
CREATE TABLE `pay_account_logs` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `account_id` int(11) NOT NULL DEFAULT '0' ,
  `balance_amount` int(11) NOT NULL DEFAULT '0' ,
  `refund_amount` int(11) NOT NULL DEFAULT '0' ,
  `amount` int(11) NOT NULL DEFAULT '0' ,
  `log_type` int(11) NOT NULL DEFAULT '0',
  `created_at` datetime NOT NULL,
  `updated_at` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `idx_created_at` (`created_at`,`id`),
  KEY `idx_account_type` (`account_id`,`log_type`,`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3212639 DEFAULT CHARSET=utf8;
```
该表上有百万条的数据，也有 关于 `account_id` 的索引，但是使用 group by 查询时，确实很慢。

```
SELECT MAX(id) max_id FROM `pay_account_logs`  WHERE (created_at <= '2015-08-14 00:00:00') GROUP BY account_id
```
平均查询时间 10sec


#### 六、不合适的order by

```
EXPLAIN SELECT SQL_NO_CACHE id,
       user_id,
       amount
FROM `trade_orders`
WHERE (settlement_time >= '2015-07-01 00:00:00'
       AND settlement_time <= '2015-09-30 23:59:59')
ORDER BY `trade_orders`.`id` ASC LIMIT 3000;

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: trade_orders
         type: index
possible_keys: idx_settlement_time
          key: PRIMARY  <--- 注意这里
      key_len: 4
          ref: NULL
         rows: 6705   <---- 注意这里
        Extra: Using where
1 row in set (0.09 sec)

ERROR:
No query specified
```

平均查询时间：40 sec

```
CREATE TABLE `trade_orders` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(11) NOT NULL DEFAULT '0' COMMENT '买家id',
  `order_id` varchar(20) NOT NULL DEFAULT '' COMMENT '订单ID',
  `amount` int(11) NOT NULL DEFAULT '0' COMMENT '订单金额',
  `settlement_time` datetime NOT NULL DEFAULT '1970-01-01 00:00:00' COMMENT '结算时间',
  `daybook_settlement_time` datetime DEFAULT '1970-01-01 00:00:00' COMMENT 'tb_order_daybook表的create时间',
  `serial_number` bigint(20) unsigned DEFAULT NULL COMMENT '条码',
  `order_type` int(11) DEFAULT NULL COMMENT '表源来源于商城哪个表(1,2,3)',
  `created_at` datetime NOT NULL,
  `updated_at` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `index_trade_orders_on_user_id_and_settlement_time_and_id` (`user_id`,`settlement_time`,`id`),
  KEY `index_trade_orders_on_order_id_and_order_type_and_id` (`order_id`,`order_type`,`id`),
  KEY `index_trade_orders_on_serial_number_and_order_type_and_id` (`serial_number`,`order_type`,`id`),
  KEY `idx_settlement_time` (`settlement_time`,`id`)
) ENGINE=InnoDB AUTO_INCREMENT=57174590 DEFAULT CHARSET=utf8 COMMENT='商城订单表'
```

优化建议：

  1、去除错误的order by 查询，记录集默认就是按照id升序排列的。

  2、使查询语句能够按照正确的方式查询

```
EXPLAIN SELECT SQL_NO_CACHE id,
       user_id,
       amount
FROM `trade_orders`
WHERE (settlement_time >= '2015-07-01 00:00:00'
       AND settlement_time <= '2015-09-30 23:59:59')
LIMIT 3000;


*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: trade_orders
         type: range
possible_keys: idx_settlement_time
          key: idx_settlement_time <-- 注意这里
      key_len: 8
          ref: NULL
         rows: 12784434 <-- 注意这里
        Extra: Using where
1 row in set (0.00 sec)

ERROR:
No query specified

```
平均查询时间：0.16 sec



#### 七、模糊查询

```
EXPLAIN SELECT `pay_detailed_logs`.*
FROM `pay_detailed_logs`
WHERE (details LIKE '%waiting%'
       AND created_at < '2015-08-09 03:40:35')
  AND (`pay_detailed_logs`.`id` > 297273949)
ORDER BY `pay_detailed_logs`.`id` ASC LIMIT 10000\G

+----+-------------+-------------------+-------+---------------+---------+---------+-----+---------+-------------+
| id | select_type | table             | type  | possible_keys | key     | key_len | ref | rows    | Extra       |
+----+-------------+-------------------+-------+---------------+---------+---------+-----+---------+-------------+
| 1  | SIMPLE      | pay_detailed_logs | range | PRIMARY       | PRIMARY | 4       |     | 3340552 | Using where |
+----+-------------+-------------------+-------+---------------+---------+---------+-----+---------+-------------+

```
平均查询时间：7.5889787673950195    sec


```
EXPLAIN SELECT `editing_logs`.`loggable_id`
FROM `editing_logs`
WHERE (user_name LIKE '%折800运营linming%'
       AND action_type_id = 0
       AND loggable_type = 'TaoJifenDealBase') \G;


*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: editing_logs
         type: ref
possible_keys: by_loggable_type
          key: by_loggable_type
      key_len: 92
          ref: const
         rows: 684252
        Extra: Using where
1 row in set (10.61 sec)

15738 rows in set (3 min 45.75 sec)

 CREATE TABLE `editing_logs` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `loggable_type` varchar(30) NOT NULL DEFAULT '' ,
  `loggable_id` int(11) NOT NULL DEFAULT '0' ,
  `user_id` int(11) NOT NULL DEFAULT '0' ,
  `user_name` varchar(24) NOT NULL DEFAULT '' ,
  `create_time` datetime NOT NULL DEFAULT '1970-01-01 00:00:00' ,
  `execution_type_id` tinyint(4) NOT NULL DEFAULT '0' ,
  `action_type_id` tinyint(4) NOT NULL DEFAULT '0' ,
  `from_url` varchar(200) NOT NULL DEFAULT '' ,
  `updated_changes` longtext ,
  `tb_shop_id` int(11) NOT NULL DEFAULT '0' ,
  PRIMARY KEY (`id`),
  KEY `index_editing_logs_on_loggable_id_and_loggable_type` (`loggable_id`,`loggable_type`),
  KEY `idx_user_name` (`user_name`,`action_type_id`,`id`),
  KEY `index_editing_logs_on_create_time` (`create_time`),
  KEY `by_tb_shop_id` (`tb_shop_id`,`id`),
  KEY `by_loggable_type` (`loggable_type`,`id`)
) ENGINE=InnoDB AUTO_INCREMENT=42833047 DEFAULT CHARSET=utf8



```

优化建议：

  1、调整查询语句顺序

  2、添加合适的索引。删除旧的索引。

```
alter table editing_logs drop index `by_loggable_type`;
Query OK, 0 rows affected (3 min 12.71 sec)


alter table editing_logs add index `idx_of_loggable_type_and_type_id` (`loggable_type`, `action_type_id`, `id`);
Query OK, 0 rows affected (6 min 5.70 sec)



EXPLAIN SELECT SQL_NO_CACHE `editing_logs`.`loggable_id`
FROM `editing_logs`
WHERE (loggable_type = 'TaoJifenDealBase' AND action_type_id = 0 AND user_name LIKE '折800运营linming%') \G;

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: editing_logs
         type: range
possible_keys: idx_user_name,idx_of_loggable_type_and_type_id
          key: idx_user_name
      key_len: 75
          ref: NULL
         rows: 39546
        Extra: Using where
1 row in set (0.01 sec)
```

平均查询时间： 0.13 sec  效率提升 1700 倍



#### 八、检索条件，没有按照索引列查询

索引，中范围查询什么的，是否能使用到索引，详细演示一下

```
SELECT  deal_id, sum(sales) as all_sales
FROM `deal_daily_sales`
WHERE `deal_daily_sales`.`status` = 1
AND `deal_daily_sales`.`tag_id` IN (xxx,)
GROUP BY deal_id ORDER BY all_sales desc LIMIT 100 OFFSET 0
```

平均查询时间：20sec

```
CREATE TABLE `deal_daily_sales` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `deal_id` int(11) NOT NULL COMMENT '对应deal的id',
  `tag_id` int(11) NOT NULL COMMENT '分类id',
  `sales` int(11) NOT NULL DEFAULT '0' COMMENT '销量',
  `status` int(11) NOT NULL DEFAULT '0' COMMENT '状态1在线 -1下线',
  `date` date NOT NULL COMMENT '日期',
  `created_at` datetime NOT NULL,
  `updated_at` datetime NOT NULL,
  `refund_count` int(11) NOT NULL DEFAULT '0' COMMENT '退款的商品数量',
  `actual_total_sales_count` int(11) NOT NULL DEFAULT '0' COMMENT '当天纯粹的订单量，为日销量 + 退款的商品数量',
  PRIMARY KEY (`id`),
  UNIQUE KEY `deal_id_date_tag_idx` (`deal_id`,`date`,`tag_id`),
  KEY `tag_id_date` (`tag_id`,`date`) COMMENT '增加where条件字段的索引',
  KEY `idx_of_date_and_status` (`date`,`status`,`sales`,`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6095800 DEFAULT CHARSET=utf8 COMMENT='商品日销量表';
```

调整下SQL语句的顺序，并且添加合适的索引，瞬间减少查询时间。

```
explain SELECT SQL_NO_CACHE  deal_id, sum(sales) as all_sales
FROM `deal_daily_sales`
WHERE `deal_daily_sales`.`tag_id` IN (xxxx,xxxx,xxxx)
AND `deal_daily_sales`.`status` = 1
GROUP BY deal_id ORDER BY all_sales desc
LIMIT 100 OFFSET 0\G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: deal_daily_sales
         type: range
possible_keys: tag_id_date,idx_of_tag_id_status_deal_id
          key: idx_of_tag_id_status_deal_id
      key_len: 8
          ref: NULL
         rows: 94380
        Extra: Using where; Using temporary; Using filesort
1 row in set (0.06 sec)
```

平均查询时间： 100 rows in set (0.43 sec)





#### 九、根本没有索引


```
explain select tb_shop_id from guang_deal_outs group by tb_shop_id;
+----+-------------+-----------------+------+---------------+------+---------+------+----------+---------------------------------+
| id | select_type | table           | type | possible_keys | key  | key_len | ref  | rows     | Extra                           |
+----+-------------+-----------------+------+---------------+------+---------+------+----------+---------------------------------+
|  1 | SIMPLE      | guang_deal_outs | ALL  | NULL          | NULL | NULL    | NULL | 69008793 | Using temporary; Using filesort |
+----+-------------+-----------------+------+---------------+------+---------+------+----------+---------------------------------+
1 row in set (0.00 sec)

平均查询时间：3 sec

CREATE TABLE `guang_deal_outs` (
  `id` int(11) NOT NULL AUTO_INCREMENT ,
  `tb_shop_id` int(11) NOT NULL DEFAULT '0' ,
  `deal_id` int(11) NOT NULL DEFAULT '0' ,
  `count` int(11) NOT NULL DEFAULT '0' ,
  `deduct_id` int(11) NOT NULL DEFAULT '0' ,
  `added_count` int(11) NOT NULL DEFAULT '0' ,
  `st_date` date NOT NULL DEFAULT '1970-01-01' ,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  `amount` int(11) NOT NULL DEFAULT '0' ,
  PRIMARY KEY (`id`),
  KEY `idx_guang_dlout_dlid` (`deal_id`),
  KEY `idx_guang_dlout_cat` (`created_at`),
  KEY `idx_guang_dlout_stdate` (`st_date`)
) ENGINE=InnoDB AUTO_INCREMENT=138022946 DEFAULT CHARSET=utf8;

```

优化建议：

  1、添加必要的索引

```
mysql> alter table guang_deal_outs add index `idx_of_tb_shop_id` (`tb_shop_id`, `id`);
Query OK, 0 rows affected (7 min 41.47 sec)
Records: 0  Duplicates: 0  Warnings: 0

explain select tb_shop_id from guang_deal_outs group by tb_shop_id\G;

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: guang_deal_outs
         type: range
possible_keys: NULL
          key: idx_of_tb_shop_id
      key_len: 4
          ref: NULL
         rows: 201
        Extra: Using index for group-by
1 row in set (0.01 sec)

ERROR:
No query specified


平均查询时间：
    4818 rows in set (0.03 sec)
```

不过针对大表而言，对于一些常用的查询，可以单独建立小表，在关联的小表上进行查询。


#### 十、EXPLAIN 返回type 为 ALL，全表扫描


```

EXPLAIN SELECT `raffle_records`.*
FROM `raffle_records`
WHERE (create_time >= '2015-08-13'
       AND create_time < '2015-08-14'
       AND device_id !="0")\G;

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: raffle_records
         type: ALL  <-- 看这里
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 10753575 <- 看这里
        Extra: Using where
1 row in set (0.01 sec)

ERROR:
No query specified


```

优化建议：

  1、添加关于  create_time 和 device_id 的联合索引。


  2、最好的方式就是单独单独建立统计表，针对每天的日志情况做统计。避免在大表上进行范围查询。

```
alter table raffle_records add index `idx_of_create_time` (`create_time`, `device_id`, `id`);
Query OK, 0 rows affected (4 min 14.59 sec)


EXPLAIN SELECT `raffle_records`.*
FROM `raffle_records`
WHERE (create_time >= '2015-08-13'
       AND create_time < '2015-08-14'
       AND device_id !="0")\G;

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: raffle_records
         type: range
possible_keys: idx_of_create_time <-- 看这里
          key: idx_of_create_time
      key_len: 777  <-- 看这里
          ref: NULL
         rows: 1
        Extra: Using where
1 row in set (0.00 sec)

```

另一个例子

```
EXPLAIN SELECT `tao_search_statistics`.*
FROM `tao_search_statistics`
ORDER BY nature_total DESC LIMIT 10\G;

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tao_search_statistics
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 66493186  <- 返回值太多，坏味道
        Extra: Using filesort <- 避免使用排序
1 row in set (0.01 sec)
```
平均查询时间：13 sec

优化建议：

  1、添加 关于 nature_total 的索引


```
mysql> alter table tao_search_statistics add index `idx_of_nature_total` (`nature_total`, `id`);
Query OK, 0 rows affected (7 min 48.27 sec)
Records: 0  Duplicates: 0  Warnings: 0

EXPLAIN SELECT `tao_search_statistics`.*
FROM `tao_search_statistics`
ORDER BY nature_total DESC LIMIT 10\G;

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tao_search_statistics
         type: index
possible_keys: NULL
          key: idx_of_nature_total
      key_len: 8
          ref: NULL
         rows: 10
        Extra:
1 row in set (0.01 sec)

ERROR:
No query specified

```
平均查询时间：0.01 sec


**参考**

[慢查询](http://baike.baidu.com/link?url=QXschAZDu6WlaHS5HzbculjUAeOG09BiVshbRXoF39R1OURTGQitb-4n98TaqkBBSGRFm5PLQDrm2Tdu4joGU_)

EXPALIN的用法
--------------

### 来个栗子:

```
EXPALIN SELECT COUNT(*) FROM `table_test_a` INNER JOIN table_test_b ON table_test_b.id = table_test_a.imageable_id AND table_test_a.imageable_type = 'table_type' INNER JOIN deal_infos ON deal_infos.deal_id = table_test_b.id AND deal_infos.brand_id <= 0 WHERE `table_test_a`.`is_delete` = 0 AND `table_test_a`.`image_type` = 0 AND `table_test_a`.`imageable_type` = 'table_type' AND (table_test_b.source_type = 1);

```

### 栗子的结果：

+----+-------------+---------------------+--------+----------------------+---------------+---------+-----------------------------------------+--------+-------------+
| id | select_type | table               | type   | possible_keys        | key           | key_len | ref                                     | rows   | Extra       |
+----+-------------+---------------------+--------+----------------------+---------------+---------+-----------------------------------------+--------+-------------+
+----+-------------+---------------------+--------+----------------------+---------------+---------+-----------------------------------------+--------+-------------+

### 结果的各个属性的含义：

**id**：select查询的序列号

**select_type**：select查询的类型，主要是区别普通查询和联合查询、子查询之类的复杂查询。

  - simple: 表示简单的select，没有union和子查询
  - primary: 最外面的select,在有子查询的语句中，最外面的select查询就是primary
  - union: union语句的第二个或者说是后面那一个
  - dependent union: UNION中的第二个或后面的SELECT语句，取决于外面的查询
  - union result: UNION的结果,如上面所示

**table**：输出的行所引用的表。

**type**：联合查询所使用的类型。
  type显示的是访问类型，是较为重要的一个指标，结果值从好到坏依次是：

  system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL

  一般来说，得保证查询至少达到range级别，最好能达到ref。

  - system: 表仅有一行，这是const类型的特列，平时不会出现，这个也可以忽略不计
  - const: 表最多有一个匹配行，const用于比较primary key 或者unique索引。因为只匹配一行数据，所以很快.
    一定是用到primary key 或者unique，并且只检索出两条数据的 情况下才会是const.
  - eq_ref: 对于eq_ref的解释，mysql手册是这样说的:"对于每个来自于前面的表的行组合，从该表中读取一行。这可能是最好的联接类型，除了const类型。它用在一个索引的所有部分被联接使用并且索引是UNIQUE或PRIMARY KEY"。eq_ref可以用于使用=比较带索引的列。
  - 对于每个来自于前面的表的行组合，所有有匹配索引值的行将从这张表中读取。如果联接只使用键的最左边的前缀，或如果键不是UNIQUE或PRIMARY KEY（换句话说，如果联接不能基于关键字选择单个行的话），则使用ref。如果使用的键仅仅匹配少量行，该联接类型是不错的。
  - ref_or_null: 该联接类型如同ref，但是添加了MySQL可以专门搜索包含NULL值的行。在解决子查询中经常使用该联接类型的优化。

  **上面这五种情况都是很理想的索引使用情况**

  - index_merge: 该联接类型表示使用了索引合并优化方法。在这种情况下，key列包含了使用的索引的清单，key_len包含了使用的索引的最长的关键元素。
  - range:  给定范围内的检索，使用一个索引来检查行
  - index:  该联接类型与ALL相同，除了只有索引树被扫描。这通常比ALL快，因为索引文件通常比数据文件小。（也就是说虽然all和Index都是读全表，但index是从索引中读取的，而all是从硬盘中读的）
    当查询只使用作为单索引一部分的列时，MySQL可以使用该联接类型。
  - ALL: 对于每个来自于先前的表的行组合，进行完整的表扫描。如果表是第一个没标记const的表，这通常不好，并且通常在它情况下很差。通常可以增加更多的索引而不要使用ALL，使得行能基于前面的表中的常数值或列值被检索出。

**possible_keys**：指出MySQL能使用哪个索引在该表中找到行。如果是空的，没有相关的索引。这时要提高性能，可通过检验WHERE子句，看是否引用某些字段，或者检查字段不是适合索引。

**key**：显示MySQL实际决定使用的键。如果没有索引被选择，键是NULL。

**ken_len**：显示MySQL决定使用的键长度。如果键是NULL，长度就是NULL。文档提示特别注意这个值可以得出一个多重主键里mysql实际使用了哪一部分。

**ref**：显示哪个字段或常数与key一起被使用。

**rows**：这个数表示mysql要遍历多少数据才能找到，在innodb上是不准确的。数值越大越不好，说明没有用好索引。

**Extra**： 该列包含MySQL解决查询的详细信息。

  - using index: 只使用索引树中的信息而不需要进一步搜索读取实际的行来检索表中的信息。这个比较容易理解，就是说明是否使用了索引
  - using where: WHERE子句用于限制哪一个行匹配下一个表或发送到客户。除非你专门从表中索取或检查所有行，如果Extra值不为Using where并且表联接类型为ALL或index，查询可能会有一些错误.
  - impossible where: 表示用不着where，一般就是没查出来啥。
  - using filesort: MySQL需要额外的一次传递，以找出如何按排序顺序检索行。
  - using temporary: 为了解决查询，MySQL需要创建一个临时表来容纳结果。

  ** Using filesort或者Using temporary的话会很吃力，WHERE和ORDER BY的索引经常无法兼顾，如果按照WHERE来确定索引，那么在ORDER BY时，就必然会引起Using filesort，这就要看是先过滤再排序划算，还是先排序再过滤划算。**
