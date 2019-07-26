---
layout:     post
title:      技术栈2 - MySQL(3) - 表结构设计规范
category: stack
tags: [stack]
excerpt: 
---

MySQL表设计规范
==============

该文档涵盖了使用MySQL中建立表结构的一些规范和建议

阅读完该文档后，您将会了解到:

* 表设计
* 索引设计
* 字段类型设计

--------------------------------------------------------------------------------

NOTE: 该部分的设计规范, 指的是`业务逻辑层面`的设计, 即如何通过分析业务逻辑来设置表结构.

表设计
------
### 按照不同的业务模块和模块之间关联关系设计表
模块: 模块是指不同的业务属性, 如 用户, 帖子, 用户登陆信息等, 均属于不同的模块
关联关系: 指模块和模块之间的关联关系 常见的关联关系有三种

- 一对一
- 一对多
- 多对多

设计表一个非常重要的点为: 正确地拆分模块, 梳理模块和模块之间的关系.

#### 例子: 论坛系统设计

> 现需要开发一个论坛系统, 用户可以在论坛中发帖, 每个帖子可以进行留言

上述需求包括两个模块

- 用户
- 帖子

关联关系如下:

```
# 每个帖子都属于一个用户
# 一个用户有多个帖子
# 用户和 帖子 通过 用户id 进行关联

用户 一  <----> 多 帖子
```

根据上述两个模块和关联关系, 设计表结构如下:

```mysql
CREATE TABLE `users` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `user_name` varchar(255) COMMENT '用户名',
  )

CREATE TABLE `posts` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `content` varchar(255) COMMENT '用户内容',
  `comments` text COMMENT '留言',
  `user_id` int(11) COMMENT '创建该帖子的用户的id',
  `created_at` datetime COMMENT '帖子创建时间',
  `updated_at` datetime COMMENT '帖子最后的编辑时间'
  )
```

上述设计可以满足基本的论坛需求, 但是有一个问题: 帖子虽然有留言，
但是不知道是哪一个用户创建的留言, 也不知道是什么时间创建的留言.

所以, 上述需求应该包括三个模块:

- 用户
- 帖子
- 留言

关联关系如下:

```
# 每个帖子都属于一个用户
# 一个用户有多个帖子
# 用户和 帖子 通过 用户id 进行关联
用户 一  <----> 多 帖子

# 每个留言都属于一个用户
# 一个用户有多个留言
# 留言 和 用户 通过 用户id 进行关联
用户 一  <----> 多 留言

# 每个留言都属于一个用户
# 一个帖子有多个留言
# 留言和 帖子 通过 用户id 进行关联
帖子 一  <----> 多 留言
```

根据上述三个模块和关联关系, 可以得到表结构如下:

```mysql
CREATE TABLE `users` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `user_name` varchar(255) COMMENT '用户名',
  )

CREATE TABLE `posts` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `content` varchar(255) COMMENT '用户内容',
  `user_id` int(11) COMMENT '创建该帖子的用户的id',
  `created_at` datetime COMMENT '帖子创建时间',
  `updated_at` datetime COMMENT '帖子最后的编辑时间'
  )

CREATE TABLE `comments` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `content` varchar(255) COMMENT '留言内容',
  `user_id` int(11) COMMENT '创建该帖子的用户的id',
  `post_id` int(11) COMMENT '帖子id',
  `created_at` datetime COMMENT '留言的创建时间'
  )
```

#### 例子: 公交站系统设计
> 需求: 设计一套公交站系统, 包括公交车和公交站点

INFO: 这是一个 关联关系为`多对多`的例子

模块:

- 公交车
- 公交站点

关联关系

```
# 每个公交车有多个公交站点
# 每个公交站点有多个公交车
公交车 n  <----> n 公交站点
```

分析上面的关联关系, 可知 公交车 和 公交车所经过的 公交站点 是多对多的关系, 如

1. "马甸桥北" 这个站点 有很多个公交车通过, 如'315路, 13路';
2. "13路" 这个公交 会经过很多 公交站点, 如"马甸桥北, 马甸桥南"

对于此类多对多关系的表, 应该设置中间表: 公交车-公交站点表. 故表结构设计如下:

```mysql
CREATE TABLE `buses` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `name` varchar(255) COMMENT '公交车号'
  )

CREATE TABLE `stations` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `location` varchar(255) COMMENT '公交站点名',
  )

CREATE TABLE `buses_stations` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `bus_id` varchar(255) COMMENT '公交id',
  `station_id` varchar(255) COMMENT '公交站点id',
  )
```

WARNING: 对于多对多的关系, 一定要设置中间表的方式, 一种的错误的设计为

```mysql
CREATE TABLE `buses` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `station_ids` varchar(255) COMMENT '公交站点id, 以","分隔,
  `name` varchar(255) COMMENT '公交车号'
  )

CREATE TABLE `stations` (
  `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY,
  `location` varchar(255) COMMENT '公交站点名',
  )
```

上面的设计中, 关联关系是通过 buses 表的 `station_ids` 关联的,
这种关联方式无法方便地通过 公交站点id 方便地查询到 对应的 公交车.
只能通过 模糊查询 公交车(buses)表字段的 `station_ids` 字段来查询, 上述表结构
设计是一种不好的设计.

### 按字段的更新频率来设计表
按照字段的更新频率设计表, 是指

1. 将不怎么变化的字段放在一张表中
2. 将经常变化的字段放在另外一张表中

#### 例子: 用户登录信息表设计
> 需求: 记录用户最后一次登录的时间和登录IP

上述场景比较简单, 可以在 原有`users`表添加 2个字段

- 最后一次登录时间
- 最后一次登录IP

```mysql
ALTER TABLE `users`
  ADD `last_login_time` datetime COMMENT '最后一次登录时间',
  ADD `last_login_ip` varchar(255) COMMENT '最后一次登录IP'
```

上述两个字段经常变化, 每次用户登录都会更新上述两个值; 而用户的基本信息如 用户名, 用户昵称
等信息基本不会变化，如果用户数量级很大, 必将导致 users 表经常被更新, 从而影响users表的读写性能.

正确的方式为: 将登陆信息放置另外一张表, 即认为上述需求包括两个模块:

模块:

- 用户
- 登陆信息表

关联关系:

```
# 每个用户都有一个登陆信息
# 一个登陆信息对应一个用户
# 用户和 登陆信息 通过 用户id 进行关联
用户 一  <----> 一 登陆信息
```

```mysql
CREATE TABLE `last_login_info` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT 'tags表的id',
  `login_time` datetime COMMENT '最后一次登陆时间',
  `login_ip` varchar COMMENT '最后一次登陆IP',
  PRIMARY KEY (`id`)
)
```

索引
----
设计表结构的另外一个重要的课题就是 如何给表设计合适的索引, 构建索引包括下面两部分的内容

- 建立唯一性索引约束, 保证数据的合法性
- 建立利于查询的索引

### 用于约束的唯一性索引
唯一性索引约束是指 一个字段的值在整张表中只能出现一次.

#### 非空的唯一性索引

非空的唯一性索引是指 被索引的列 在表中唯一, 且不能为NULL
例如下面的这个需求

```
设计一个用户表, 要求: 手机号码不为空, 而且手机号不能重复
```

故上述需求应该在数据库中建立唯一性索引

```mysql
  CREATE TABLE `users` (
    `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '角色表',
    `phone_number` varchar(255) NOT NULL DEFAULT '' COMMENT '手机号码',
    UNIQUE KEY `idx_phone_number` (`phone_number`)
  ) ENGINE=InnoDB
```

#### 可以为空的唯一性索引
考虑下面一种场景

```
用户现在可以有两种方式进行注册, 一种是手机号, 一种是邮箱, 用户可以通过
上述两种方式的任意一种进行注册, 但无论是哪一种, 手机号和邮箱必须要唯一
```

此时需要分别对 手机号, 邮箱 添加唯一性索引, 不填则存储NULL, 即

```mysql
  CREATE TABLE `users` (
    `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '角色表',
    `phone_number` varchar(255) COMMENT '手机号码',
    `email` varchar(255) COMMENT '邮箱',
    UNIQUE KEY `idx_phone_number` (`phone_number`),
    UNIQUE KEY `idx_email` (`email`)
  ) ENGINE=InnoDB
```

WARNING: 对于重要的表如 users, 一些约束一定要放在数据库层面, 而不是服务器层面;

#### 联合唯一性索引
> 假设现在有一个用户表, 通过手机号可进行注册, 并要求手机号必须唯一;
另外, 用户有一个删除状态: delete_status, 该字段标示这个用户是否被删除.
如果用户被标记删除, 则手机号可以重新使用.

即 users表的字段为

- id
- phone_number
- delete_status

此时, 不能仅仅建一个 phone_number 的索引, 而应该建一个 phone_number delete_status 的联合索引, 即:

```mysql
ALTER TABLE users ADD INDEX `idx_phone_number_delete_status` (`phone_number`, `delete_status`)
```

### 用于查询的索引
#### 哪些字段需要建索引
1. 经常使用的字段
如 `users.user_name`

2. 关联表所用到的字段
如: 设计论坛系统, 论坛中的 帖子 表 posts 中字段如下

- id
- content
- user_id

其中 user_id 为关联users的外键字段, 需要添加索引

#### 哪些字段不需要建索引
1. 区分度很低的字段
假设有一个 users.delete_status字段, 表示该用户是否被删除, 该字段的值只有两个0, 1. 该字段就不应该建立索引, 因为区分度太低, 索引的效率很差
2. 类型为 text 的字段不应该建索引
3. 类型为 varchar, char 的字段的索引长度不要超过10

字段类型使用规范
-----------
### 可选的字段类型
MySQL的字段类型有很多, 但我们只能下面的几个类型的字段

- integer  整数类型
- varchar  字符串类型
- datetime 时间类型
- date     日期类型
- text     文本类型

其他类型 如 float, enum 等禁止使用

### 一些字段设计的建议
- 如果需要存小数, 则考虑通过乘以 10^^n 来转变成整数.
  例如存储金额1.32元, 不能存一个float类型的数字1.32, 而应该传一个整数132
- 不要使用枚举类型, 用 integer 类型替代 枚举类型.
  例如存储"apple", "orange", "banana"三种水果, 在数据库里面应存储 0,1,2 的integer类型。
- 对于 NOT NULL 的字段, 需要设置一个默认值
- 对于 可以为 NULL 的字段, 不要设置默认值
- 当字段为 integer, varchar 的时候, 需要考虑存储的值是否会溢出
