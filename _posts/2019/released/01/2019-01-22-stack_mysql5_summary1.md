---
layout:     post
title:      技术栈2 - MySQL(5) - mysql学习总结1
category: stack
tags: [stack]
excerpt: 
---

mysql学习总结
=============

初级
-----------

+ 数据类型，

[data-types](https://dev.mysql.com/doc/refman/5.5/en/data-types.html)


+ 创建表，create table
+ 修改表，alter table add|drop|modify
+ 主键策略，auto_increment, uuid
+ 面向列和面向行的数据库
+ 一、二、三范式，反范式

[解释一下关系数据库的第一第二第三范式](http://www.zhihu.com/question/24696366)
[数据库系统概论](http://item.jd.com/11550202.html)

+ DML、DDL、DCL
+ 检索，SELECT,LIMIT, AS, ORDER BY, ASC, DESC
+ 数据库的操作符 =, <>, !=, <, <=, >, >=, BETWEEN
+ MySQL 的关键字

[MySQL 关键字](http://www.cnblogs.com/lawdong/archive/2010/08/08/2357903.html)

+ MySQL 空值 NULL

[尽量避免使用NULL](https://ruby-china.org/topics/26873)

+ 过滤符, AND, OR, IN, NOT

+ 正则匹配

《 MySQL 必知必会》p52

+ 字符串函数
Concat(),  RTrim(), AS, Upper(), Left(), Length(), Locate(), Lower(), LTrim(), Right(), RTrim(), Soundex(), SubString()

[functions](https://dev.mysql.com/doc/refman/5.5/en/functions.html)

+ 日期和时间处理函数
AddDate(), AddTime(), CurDate(), CurTime(), Date(), DateDiff(), Date_Add(), Date_Format(), Day(), DayOfWeek(), Hour(), Minute(), Month(), Now(), Second(), Time(), Year()

[functions](https://dev.mysql.com/doc/refman/5.5/en/functions.html)


+ 数值处理函数
	Abs(), Cos(), Exp(), Mod(), Pi(), Rand(), Sin(), Sqrt(), Tan()

[functions](https://dev.mysql.com/doc/refman/5.5/en/functions.html)


+ 汇总数据函数
	AVG(), COUNT(), MAX(), MIN(), SUM()

[functions](https://dev.mysql.com/doc/refman/5.5/en/functions.html)

+ 子查询

+ 表连接， 内连接 等值连接，左连接， 右连接，全连接， 自连接

[difference-between-inner-and-outer-joins](http://stackoverflow.com/questions/38549/difference-between-inner-and-outer-joins)

联结是一种机制，用来在一条SELECT语句中关联表，因此称之为联结。使用特殊的语法，可以联结多个表返回一组输出，联结在运行时关联表中正确的行。

关于外键。

+ 笛卡尔积


+ 组合查询 UNION
+ 数据插入，完成插入，插入多行，插入检索数据，数据更新与删除。

+ 视图

	- 重用SQL语句
	- 简化复杂的SQL操作。在编写查询后，可以方便地重用它而不必知道它的基本查询细节。
	- 使用表的组成部分而不是整个表。
	- 保护数据。
	- 更改数据格式和表示。视图可以返回与底层表的表示和格式不同的数据。

《MySQL必知必会》p208

+ 存储过程

《MySQL必知必会》	p163

+ 触发器，INSERT 触发器，DELETE触发器，UPDATE触发器，DELETE 触发器

《MySQL必知必会》p181

+ MySQL 字符集及存储范围

[MySQL 数据库 varchar 到底可以存多少个汉字，多少个英文呢?我们来搞搞清楚](https://ruby-china.org/topics/24920)

+ 管理事务处理

《MySQL必知必会》p249

进阶
---------------

《高性能MySQL》
《MySQL技术内幕：InnoDB存储引擎》
《数据库系统概论》
《数据库查询优化器的艺术》

+ 索引覆盖率
+ 索引锁定
+ MySQL 查询优化器
+ 顺序读取与随机读取的区别
+ 机械硬盘和SSD 在使用上优缺点
+ 表聚簇，索引组织表，页连接
+ 完全多余的索引，近乎多余的索引，可能多余的索引
+ 新增索引代价，响应时间，磁盘负载，磁盘空间
+ 查询时间估算
+ 为什么连接的性能比较差

 模糊的索引设计，优化器可能选择错误的表访问路径，乐观的表设计

+ 表设计，冗余，汇总
+ Btree 的实现。分裂，索引的稳定性
+ MySQL 参数设置 与查询优化的关系
+ 成本估算
 估算 I/O时间，估算 CPU 时间

+ InnoDB 关键特性

插入缓冲，两次写，自适应哈希索引，异步IO，刷新领接页

+ InnoDB 逻辑存储结构

+ 分布式表，分布式事务

+ MySQL 备份与恢复，主从架构

+ MySQL 与 HTTP

+	操作系统与硬件优化

	CPU 的选择，选择更多的CPU 还是更快的CPU
	随机I/O 和顺序I/O
	如何选择硬盘
	RAID性能优化

+ 可扩展，高可用的MySQL 设计与实现

+ 应用层优化

	寻找最优并发度
	缓存控制与替代

