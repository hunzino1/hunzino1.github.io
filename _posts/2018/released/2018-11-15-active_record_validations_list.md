---
layout:     post
title:      Active Record Validations
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

Active Record Validations
=========

该文档列出了Rails Active Record Validations的基本知识点.

阅读完该文档后，您将会了解到:

* 为什么使用Active Record Validations.
* Active Record Validations主要内容概括.

--------------------------------------------------------------------------------

> Validations are used to ensure that only valid data is saved into your database.

Active Record Validations知识总览
------------------
[官方文档](http://guides.rubyonrails.org/active_record_validations.html)

  + 什么是validate
  + validate发生在什么地方
  + 绕过校验 validate: false
  + 校验是如何触发的?
  + 校验的几种形式
    * validates_XXX_of， 可校验的形式有:
         ```
acceptance
validates_associated
confirmation
exclusion
format
inclusion
length
numericality
presence
absence
uniqueness
         ```
    * validates 字段名, presences: true, ...
    * 自定义的validate, 如 validate :check_deal_exist
  + 比较重要的校验参数
    * :on          => :create
    * :if          => { |object| object.check_some_condition }
    * :allow_blank => true
    * :allow_nil   => true
    * :message     => "deal已经存在"
    * :scope         => [:deal_id]
  + 校验后的错误信息 errors
    * errors是如何产生的
    * errors中的字段的i18n配置
    * errors.add(:base, 'msg')
    * errors.add('某个字段', 'msg')
    * errors.full_messages
  + Rails的唯一性校验 和数据库唯一性索引校验的区别
  + 添加校验后应该注意历史数据的问题
