---
layout:     post
title:      技术栈2 - MySQL(9) - 面试问题汇总
no-post-nav: true
category: stack
tags: [stack]
excerpt: 
---

《高性能MySQL》
《数据库索引设计与优化》


+ 事务处理《MySQL 必知必会》 p249， 隔离级别 《MySQL技术内幕：InnoDB存储引擎 第7章》

+ 主键策略，auto_increment, UUID 适用范围和优缺点


+ 锁，读写锁，表级锁，列级锁，锁粒度，死锁
《MySQL技术内幕：InnoDB存储引擎 第6章》

+ Drop 和 Truncate 的区别
+ 各种主键策略的优缺点，及用处
+ 数据库引擎，InnoDB, MyISAM 优缺点
+ MySQL 性能分析，Explain的用法，调优的基本参数
+ Schema与数据类型优化，varchar 和 char 的优缺点，不同日期类型的区间及如何格式化
+ 从网络，内存，磁盘分析SQL操作为什么慢
+ 索引是什么，索引为什么能加速查询。索引的优点与缺点
+ 索引分类，前缀索引，多列索引，聚簇索引，二级索引，覆盖索引，主键索引，哈希索引
+ 熟悉Btree
+ 使用索引优化慢查询

	优化 COUNT()查询，优化关联查询，优化子查询，优化GROUP BY 和 DISTINCT，优化LIMIT分页，优化UNION查询

+ 索引的设计与优化，一星索引，二星索引，三星索引
+ 表结构设计与优化
+ 连接MySQL

	TCP/IP
	命名管道和共享内存
	UNIX域套接字

+ 连接池是什么，连接池如何优化
+ InnoDB 体系架构
+ Cardinality值 《MySQL技术内幕：InnoDB存储引擎 5.5》
+ 分区

	分区表的原理，
	分区表的类型
	如何使用分区表，
	分区表留下的坑

+ 视图

	视图创建，视图对性能的影响

+ 查询缓存

	如何命中查询缓存
	提高查询缓存的使用率
	查询缓存的配置与维护

+ MySQL 监控和测试工具使用

