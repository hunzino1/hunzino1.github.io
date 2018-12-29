---
layout:     post
title:      Action View Form Helpers
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

Action View Form Helpers
==============================

该文档列出了 Action View Form Helpers 的基本知识点.

阅读完该文档后，您将会了解到:

* Action View Form Helpers 主要内容概括

--------------------------------------------------------------------------------

Action View Form Helpers 知识总览
---------------------------------------------------

[官方文档](http://guides.rubyonrails.org/form_helpers.html)

### 基础Form表单的Helper方法
- form_tag方法及其选项
  + :method
  + :controller
  + :action
  + :class, :style, :id, ...

- label_tag
- text_field_tag, text_area_tag
- submit_tag
- check_box_tag
- radio_button_tag
- password_field_tag
- hidden_field_tag
- file_field_tag

### 处理Model对象对应Form表单的Helper方法
- 把基础的Helper方法的"_tag"后缀去掉。
- form_for方法及其选项
  + :url
  + :html
- fields_for方法

### 下拉框
- select_tag 方法
- options_for_select 方法

### 参数命名约定

### 嵌套的表单， fields_for方法

### 参数白名单(Rails 4)
- require
- permit

