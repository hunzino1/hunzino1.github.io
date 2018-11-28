---
layout:     post
title:      Active Record Query Interface
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

Active Record Query Interface
==============================

该文档列出了Rails Active Record Query Interface的基本知识点.

阅读完该文档后，您将会了解到:

* Active Record Query Interface主要内容概括.

--------------------------------------------------------------------------------

> Active Record will perform queries on the database for you and is compatible with most database systems (MySQL, PostgreSQL and SQLite to name a few).

Active Record Query Interface知识总览
------------------
[官方文档](http://guides.rubyonrails.org/active_record_querying.html)

### 从数据库检索对象
  * 查询方法
    + bind
    + create_with
    + distinct
    + eager_load
    + extending
    + from
    + group
    + having
    + includes
    + joins
    + limit
    + lock
    + none
    + offset
    + order
    + preload
    + readonly
    + references
    + reorder
    + reverse_order
    + select
    + uniq
    + where
    + 上述方法均返回ActiveRecord::Relation 的一个实例
    + Model.find(options) 做了些什么

  * 检索单个对象
    + find
    + take
    + first
    + last
    + find_by

  * 批量检索多个对象
    + all(该方法慎用！)
    + find_each(参数： :batch_size, :start)
    + find_in_batches(参数： :batch_size, :start)

### 查询条件
  * 纯字符串条件(注意防止数据库注入的安全漏洞)
  * 数组形式的条件(占位符形式的条件)
  * Hash形式的条件
    + 相等条件
    + 区间条件
    + 子集条件(Subset Conditions)
  * NOT 条件
    + not

### 排序
 + order的用法，不同的参数形式

### 选择指定的字段
 + select
 + distinct

### limit 和 offset

### Group
 + group
 + group后的总数, Model.group.count

### having

### 覆写查询条件
  + unscope
  + only
  + reorder
  + reverse_order
  + rewhere

### Null Relation(空关系)
  + none

### 只读的对象
  + readonly

### Locking Records for Update(锁住记录)
  + Optimistic Locking(乐观锁)
    * lock_version 字段
  + Pessimistic Locking(悲观锁)

### Joining Tables(关联表)
  + joins
  + 字符串SQL片段
  + 具名关联关系的Array/Hash(该方式仅支持INNER JOIN)
    - join单个关联表
    - join多个关联表
    - join嵌套的关联表(一层嵌套)
    - join嵌套的关联表(多层嵌套)
  + 如何在被join的表上指定查询条件

### Eager Loading Associations(即时加载关联表)
  + N + 1查询问题及其解决方式
  + includes
  + 即时加载多个关联表
    * 多个关联表的数组
    * 嵌套关联表的Hash
  + 如何在及时加载的关联表上指定查询条件
  + 强制join表
    * references

### Scopes
  + 如何定义scope
  + scope本质上就是类方法
  + 传入参数
  + default_scope
  + scopes 的合并
    * Relation#merge
  + 去掉所有 Scoping
    * unscoped

### 动态查询方法
  + Find or Build a New Object(查询或者新建一个对象)
    * find_or_create_by
  + find_or_create_by!
  + find_or_initialize_by

### Finding by SQL(用SQL语句查询)
  + find_by_sql
  + select_all
    * select_all不会实例化查询结果
  + pluck
  + ids

### 对象的存在性
  + 检查一个model或relation的存在性
    * exists?(id)
    * any?
    * many?

### 计算
  + count
  + average
  + minimum
  + maximum
  + sum

### 运行 EXPLAIN
  + explain
  + 解释 EXPLAIN

