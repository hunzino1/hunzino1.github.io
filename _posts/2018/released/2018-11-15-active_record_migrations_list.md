---
layout:     post
title:      Active Record Migrations
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

Active Record Migrations
=========

该文档列出了Rails Active Record Migrations模块的基本知识点.

阅读完该文档后，您将会了解到:

* 什么是Active Record Migrations.
* Active Record Migrations模块主要内容概括.

--------------------------------------------------------------------------------

> Migrations are a convenient way to alter your database schema over time in a consistent and easy way.

 Active Record Migrations知识总览
------------------
[官方文档](http://guides.rubyonrails.org/active_record_migrations.html)

  + 如何用Rails的命令创建migration
  + Rails和数据库对应的基本的类型有哪些
  + 创建表
  + 添加索引(唯一性索引，联合索引等)
  + 添加字段
  + 修改表的某个字段的属性
  + 创建表的规范(必须添加注释, tinyint, 指定ENGINE=InnoDB等)
  + 构建测试库
     * rake db:test:clone
     * rake db:test:prepare
     * rake db:test:clone_structure
  + RAILS_ENV = test/production/development
  + rake db:migrate:up VERSION=XXXX
  + rake db:migrate:down VERSION=XXXX
  + Rails的schema_migrations表是如何管理项目中 db/migrate/ 文件
  + 执行rake db:migrate 的时候发生了什么
  + db/schema.rb文件的作用
