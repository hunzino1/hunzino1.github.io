---
layout:     post
title:      Layouts and Rendering in Rails
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

Layouts and Rendering in Rails
==============================

该文档列出了 Layouts and Rendering in Rails 的基本知识点.

阅读完该文档后，您将会了解到:

* Layouts and Rendering in Rails主要内容概括

--------------------------------------------------------------------------------

 Layouts and Rendering in Rails 知识总览
---------------------------------------------------

[官方文档](http://guides.rubyonrails.org/layouts_and_rendering.html)

- 同一个action中，render和redirect_to不能同时调用，也不能重复调用任意一个。
- Rails默认render

- render
  + an Action's View
  + :nothing
  + :file
  + :action
  + :template
  + :plain
  + :html
  + :json
  + :js
  + :xml
  + :content_type
  + :layout
  + :status

- redirect_to
  + :status

- render 和 redirect_to 的区别

- 理解yield, 理解layout的作用

- content_for

- Partials
  + :layout
  + :locals
  + :object


