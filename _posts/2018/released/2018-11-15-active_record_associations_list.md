---
layout:     post
title:      Active Record Associations
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

Active Record Associations
===========================

该文档列出了Rails Active Record Associations的基本知识点.

阅读完该文档后，您将会了解到:

* 为什么使用Active Record Associations
* Active Record Associations主要内容概括.

--------------------------------------------------------------------------------

> Why do we need associations between models? Because they make common operations simpler and easier in your code.

Active Record Associations知识总览
------------------
[官方文档](http://guides.rubyonrails.org/association_basics.html)

## 为什么使用Associations ?

## Associations的类型
  + belongs_to
  + has_one
  + has_many
  + has_many :through
  + has_one :through
  + has_and_belongs_to_many
  + 如何判断选择belongs_to还是has_one
  + 如何判断选择has_many :through 还是 has_and_belongs_to_many
  + Polymorphic Associations（多态关联, :polymorphic, :as）
  + Self Joins(自关联)

## 注意事项
  + Controlling caching(控制缓存)
  + Avoiding name collisions(避免命名冲突)
  + Updating the schema(更新数据库表结构)
    * 为belongs_to创建外键
    * 为has_and_belongs_to_many创建关联表(中间表)
  + Controlling association scope(控制关联的scope，必要时需要使用:class_name参数)
  + Bi-directional associations(双向关联,多份对象拷贝可能导致数据不同步，:inverse_of参数)

## 各个关联关系详解
### belongs_to详解
  + belongs_to 添加了哪些方法
    * association(force_reload = false)
    * association=(associate)
    * build_association(attributes = {})
    * create_association(attributes = {})
    * create_association!(attributes = {})
  + belongs_to 有哪些可选参数
    * :autosave
    * :class_name
    * :counter_cache
    * :dependent
    * :foreign_key
    * :inverse_of
    * :polymorphic
    * :touch
    * :validate
  + belongs_to 的scopes
    * where
    * includes
    * readonly
    * select
  + 判断关联对象是否存在: association.nil? 方法
  + 关联对象何时被保存

### has_one 详解
  + has_one 添加了哪些方法
    * association(force_reload = false)
    * association=(associate)
    * build_association(attributes = {})
    * create_association(attributes = {})
    * create_association!(attributes = {})
  + has_one 有哪些可选参数
    * :as
    * :autosave
    * :class_name
    * :dependent
    * :foreign_key
    * :inverse_of
    * :primary_key
    * :source
    * :source_type
    * :through
    * :validate
  + 判断关联对象是否存在: association.nil? 方法
  + 关联对象何时被保存

### has_many 详解
  + has_many 添加了哪些方法
    * collection(force_reload = false)
    * collection<<(object, ...)
    * collection.delete(object, ...)
    * collection.destroy(object, ...)
    * collection=(objects)
    * collection_singular_ids
    * collection_singular_ids=(ids)
    * collection.clear
    * collection.empty?
    * collection.size
    * collection.find(...)
    * collection.where(...)
    * collection.exists?(...)
    * collection.build(attributes = {}, ...)
    * collection.create(attributes = {})
    * collection.create!(attributes = {})
  + has_many 有哪些可选参数
    * :as
    * :autosave
    * :class_name
    * :dependent
    * :foreign_key
    * :inverse_of
    * :primary_key
    * :source
    * :source_type
    * :through
    * :validate
  + has_many 可用的 scopes
    * where
    * extending
    * group
    * includes
    * limit
    * offset
    * order
    * readonly
    * select
    * uniq
  + 关联对象何时被保存

### has_and_belongs_to_many 详解
  + has_and_belongs_to_many 添加了哪些方法
    * collection(force_reload = false)
    * collection<<(object, ...)
    * collection.delete(object, ...)
    * collection.destroy(object, ...)
    * collection=(objects)
    * collection_singular_ids
    * collection_singular_ids=(ids)
    * collection.clear
    * collection.empty?
    * collection.size
    * collection.find(...)
    * collection.where(...)
    * collection.exists?(...)
    * collection.build(attributes = {})
    * collection.create(attributes = {})
    * collection.create!(attributes = {})
  + has_many 有哪些可选参数
    * :association_foreign_key
    * :autosave
    * :class_name
    * :foreign_key
    * :join_table
    * :validate
  + has_many 可用的 scopes
    * where
    * extending
    * group
    * includes
    * limit
    * offset
    * order
    * readonly
    * select
    * uniq
  + 关联对象何时被保存

## Association Callbacks(关联Callbacks)
  + before_add
  + after_add
  + before_remove
  + after_remove

## Association Extensions(关联扩展, extending)

