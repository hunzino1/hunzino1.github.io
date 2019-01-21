---
layout:     post
title:      技术栈2 - MySQL(6) - MySQL规范
category: stack
tags: [stack]
excerpt: 
---

MySQL规范-DBA
=============

该文档涵盖了使用MySQL中需要熟记的参考规范

阅读完该文档后，您将会了解到:

* MySQL命名规范
* MySQL编写规范
* MySQL事务规范

--------------------------------------------------------------------------------

命名规范
--------
### 数据库名
库名和项目相关, 简短易懂, 全部小写, 只能使用字母, 数字, 下划线组成, 使用字母开头, 多个单词使用下划线分隔。

### 表名
- 表名和功能相关, 简短易懂, 不要使用缩写形式.

  ```mysql
  # bad
  CREATE table ord_rsl

  # good
  CREATE table order_results xxx
  ```

- 全部小写, 只能使用字母, 数字, 下划线组成, 使用字母开头, 多个单词使用下划线分隔

  ```
  # bad
  CREATE table OrderInfo
  CREATE table OrderINFO

  # good
  CREATE table order_infos
  ```

- 表名以复数单词结尾, 如`orders`

  ```mysql
  # bad
  CREATE table order

  # good
  CREATE table orders
  ```

### 字段名
- 字段名和字段存储数据相关, 全部小写, 只能使用字母, 数字, 下划线组成, 使用字母开头, 多个单词使用下划线分隔

  ```mysql
  # bad
  CREATE TABLE `messages` (
    `creatorId` int(11) NOT NULL DEFAULT '0' COMMENT '创建者id'
    xxx
  )

  # good
  CREATE TABLE `messages` (
    `creator_id` int(11) NOT NULL DEFAULT '0' COMMENT '创建者id'
    xxx
  )
  ```

- 避免使用系统关键字作为表名或者字段名

  ```mysql
  # bad 下面的字段key, group都是关键字
  CREATE TABLE `admin_authorities` (
    `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '运营后台权限表',
    `key` varchar(100) COLLATE utf8_unicode_ci NOT NULL DEFAULT '' COMMENT '权限名称',
    `desc` varchar(100) COLLATE utf8_unicode_ci NOT NULL DEFAULT '' COMMENT '权限描述',
    `label` varchar(100) COLLATE utf8_unicode_ci NOT NULL DEFAULT '' COMMENT '权限标签',
    `group` varchar(100) COLLATE utf8_unicode_ci NOT NULL DEFAULT '' COMMENT '权限所在组',
    `created_at` datetime NOT NULL,
    `updated_at` datetime NOT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `key_index` (`key`) USING BTREE
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT='运营后台权限表'
  ```

表字段规范
----------
### 字段规范
- 为每张表都设置一个自增主键(AUTO_INCREMENT PRIMARY KEY) id

  ```mysql
  # bad 没有设置主键
  CREATE TABLE `roles` (
    `name` varchar(255) COLLATE utf8_unicode_ci NOT NULL DEFAULT '' COMMENT '角色名称',
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT='后台编辑角色表'

  # good
  CREATE TABLE `roles` (
    `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '后台编辑角色表',
    `name` varchar(255) COLLATE utf8_unicode_ci NOT NULL DEFAULT '' COMMENT '角色名称',
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci COMMENT='后台编辑角色表'
  ```

- 和金额相关的字段, 应该使用integer类型, 而不是用decimal或float, 保存的单位为`分`

  ```mysql
  # bad
  CREATE TABLE `orders` (
    `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '后台编辑角色表',
    `price` decimal NOT NULL DEFAULT 0 COMMENT='订单价格, 单位元',
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci

  # good
  CREATE TABLE `orders` (
    `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '后台编辑角色表',
    `price` integer NOT NULL DEFAULT 0 COMMENT='订单价格, 单位分',
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
  ```

- 字符串使用`varchar`, 尽量不要使用`text`
- 时间类型使用`datetime`
- 用`tinyint(4)`代替枚举类型
- 非空(NOT NULL)字段需要设置默认值
- 字符集使用utf-8

SQL编写规范
-----------
- 不要使用模糊查询

  ```mysql
  # very very bad
  SELECT * FROM users WHERE user_name like %qing%

  # bad
  SELECT * FROM users WHERE user_name like %qing

  # good if user_name has index
  SELECT * FROM users WHERE user_name = 'qing'
  ```

- 不使用函数或者计算进行查询

  ```mysql
  SELECT * FROM orders WHERE (amount - 1) > 100
  ```

- JOIN部分的字段需要有索引
- WHERE子句中如果用到IN, IN中的条件数量不能超过50个
- 尽量不用OR, 相同字段用IN; 不同字段尽量用UNION(用OR不会使用索引)

事务规范
--------
- 事务需要尽快提交, 不要在事务中进行耗时操作
- 事务中操作资源的顺序应该一致, 避免死锁
- 将大事务拆分成多个小事务执行

其他
----
- 不要对text做索引
- 对超过`10`个字符串的字段, 使用最左索引, 不要全字段索引
- 一个表的索引数量不要超过字段数量的`50%`
- 每个表需要写注释(comment), 描述此表的作用和其他表的关联关系
- 单表字段数不要超过50个
- 重要数据不要物理删除, 增加逻辑删除字段
- 单次修改超过100W 条, 需要拆分成多个SQL提交

参考资料
--------
