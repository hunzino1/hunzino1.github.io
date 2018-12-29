---
layout:     post
title:      Action Controller
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

Action Controller
================

该文档列出了Rails Action Controller模块的基本知识点.

阅读完该文档后，您将会了解到:

* 什么是Action Controller.
* Action Controller模块主要内容概括.
* 路由模块主要内容概括.

--------------------------------------------------------------------------------

Action Controller知识总览
-------------------------
### Action Controller Overview

> Action Controller is the C in MVC. After routing has determined which controller to use for a request, your controller is responsible for making sense of the request and producing the appropriate output.

[官方文档](http://guides.rubyonrails.org/action_controller_overview.html)

- controler和action
  + 如何用Rails的命令创建controller
  + controller的命名规则
  + 如何从log中找到对应的controller和action

- params
  + params中包含了那些信息
  + params.require, strong params, attr_accessible
  + 为什么会有 Strong Parameters的存在

- session, flash 和 cookies
  + session有哪几种存储方式，我们项目中用的哪一种?
  + session的配置
  + session的增删改查
  + flash和session的区别
  + cookies的增删改查
  + cookies, session, flash的使用场景的区别

- filters
  + 基本的filter: before_filter, after_filter, around_filter
  + 参数 only, except

- Rails如何使用authenticity_token来防止CSRF攻击

- request和response
  + request对象和response对象的生命周期是怎么样的, 可以在哪些地方调用
  + request.referer, request.method等

- Log Filtering
  + 如何设置敏感参数(密码等)在log中不可见

- render, redirect_to和respond_to
- 如何导出文件(send_data), 如何下载已经存在的文件(send_file)

### 路由部分

> The Rails router recognizes URLs and dispatches them to a controller's action. It can also generate paths and URLs, avoiding the need to hardcode strings in your views.

[官方文档](http://guides.rubyonrails.org/routing.html)

- resources在config/routes.rb的使用
- 使用resources方法生成了哪些路由?
- resources的参数 only: , except:
- 通过`rake routes`可看到所有的路由, 该命令输出的每一列代表什么?
- 通过resource可以得到哪些可以使用的path?
- 具名路由(Routes with Namespaces)
- 声明嵌套路由(Nested Resources), 嵌套路由的使用场景是什么?
- 添加RESTful风格的路由, member, collection的使用
- member和collection是按照什么样的规则生成路由?
- 自定义路由, 如`get 'profile', to: 'users#show'`
