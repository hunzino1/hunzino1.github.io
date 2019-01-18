---
layout:     post
title:      计算机网络(1) - 网络分层、TCP、IP、Http、Socket总结
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 
---

网络分层、TCP、IP、Http、Socket总结
====================================



> 按照自身所设定的知识树学习之路，仅供参靠。

# 一) 总纲

- 1、【2天】学习[Vim](http://www.vim.org/), 请下载vim7.2中文手册
- 2、【1天】学习Linux基本命令 和 git(《git pro》)[git专版页面]()
- 3、【2天】学习Ruby, 请参考《Ruby基础教程》
- 4、【10天】学习《Ruby On Rails》

### linux基本命令：
- [Linux 新手应该知道的 26 个命令](http://locez.com/Linux/common-command/)
- [41个Linux基础命令介绍--整理自鸟哥私房菜](https://blog.csdn.net/xiaoguaihai/article/details/8705992)
- [Linux命令便捷查询手册](http://linux.51yip.com/)

# 二) ruby知识树

该文档涵盖了Ruby和Ruby On Rails从初级程序员至高级程序员, 最终成为系统架构师需要掌握的知识点.

阅读完该文档后，您将会了解到:

* Ruby和Ruby On Rails的初级，中级，高级技巧.
* Web架构设计
* 设计模式，计算机原理，数据与数据结构等

--------------------------------------------------------------------------------

Linux操作 @liukun
---------
#### [Linux常用命令列表](https://hunzino1.github.io/other/2018/Linux_basic_commands)
#### 《鸟哥私房菜》

git和git-flow @dengqinghua
-------------
#### git
* [Git常用命令列表](https://hunzino1.github.io/other/2018/git_basic_commands)
* [Git的原理(参考书籍 摘出章节)](https://hunzino1.github.io/other/2018/git_inspection)

#### git-flow
* [git-flow 工作流程和基本命令](https://hunzino1.github.io/other/2018/git_flow_basic_commands)

代码/注释规范 @dengqinghua
-------------
#### [代码规范基础](http://git.tuan800-inc.com/ruby/tuan800/wikis/wiki-code-ruby-style)
#### [代码注释规范](https://hunzino1.github.io/other/2018/code-comments-style)

Ruby语言 @dengqinghua @liukun
--------
#### 基础语法 @liukun
* [Ruby知识概要和关键词](https://hunzino1.github.io/other/2018/ruby_basic_knowledge)
* [Ruby基础知识梳理](https://hunzino1.github.io/other/2018/ruby_basic_introduction)
* 《Ruby基础教程》(编号：95)
* [Learn Ruby the hard way](http://learnrubythehardway.org/)

#### 类库
* 《Programming Ruby》(TODO 添加书名编号)
* http://ruby-doc.org/

#### 元编程 @dengqinghua
* [ruby元编程](https://hunzino1.github.io/other/2018/ruby_metaprogramming)

Rails框架
------------
#### MVC的概念
[当请求到服务器时，MVC三部分的协作方式](https://hunzino1.github.io/other/2018/mvc_overview)

#### Model
  * [Active Record Basics](https://hunzino1.github.io/other/2018/active_record_basics_list)
  * [Active Record Migrations](https://hunzino1.github.io/other/2018/active_record_migrations_list)
  * [Active Record Validations](https://hunzino1.github.io/other/2018/active_record_validations_list)
  * [Active Record Callbacks](https://hunzino1.github.io/other/2018/active_record_callbacks_list)
  * [Active Record Associations](https://hunzino1.github.io/other/2018/active_record_associations_list)
  * [Active Record Query Interface](https://hunzino1.github.io/other/2018/active_record_query_interface_list)

#### Controller
  * [Action Controller and Routes](https://hunzino1.github.io/other/2018/controller_list)

#### View
  * [Layouts and Rendering in Rails](https://hunzino1.github.io/other/2018/layouts_and_rendering_list)
  * [Action View Form Helpers](https://hunzino1.github.io/other/2018/form_helpers)
  * [前端知识](https://hunzino1.github.io/other/2018/front_list)

#### 其他
   * [Rails常用命令和Rake的用法](https://hunzino1.github.io/other/2018/command_line_list)
   * [主从库使用](https://hunzino1.github.io/other/2018/master_slave_list)
   * [Rails配置](https://hunzino1.github.io/other/2018/rails_config_list)
   * [RSpec](https://hunzino1.github.io/other/2018/rspec_list)

Rails开发相关工具
-----------------------------
#### Redis
  * [Redis基本用法](https://hunzino1.github.io/other/2018/redis_list)

#### Sidekiq
  * [Sidekiq基本用法](https://hunzino1.github.io/other/2018/sidekiq_list)

#### God
  * [God基本用法](https://hunzino1.github.io/other/2018/god_list)

#### rails_service
  * [rails_service基本用法](https://hunzino1.github.io/other/2018/rails_service_list)

#### Memcached
  * [Memcached基本用法](https://hunzino1.github.io/other/2018/memcached_list)

#### Ngnix
  * [Nginx基本用法](https://hunzino1.github.io/other/2018/nginx_list)

Rails Guides
---------------
#### 必读部分
- Models
  + [Getting Started with Rails](http://guides.rubyonrails.org/getting_started.html)
  + [Active Record Basics](http://guides.rubyonrails.org/active_record_basics.html)
  + [Active Record Migrations](http://guides.rubyonrails.org/active_record_migrations.html)
  + [Active Record Validations](http://guides.rubyonrails.org/active_record_validations.html)
  + [Active Record Callbacks](http://guides.rubyonrails.org/active_record_callbacks.html)
  + [Active Record Associations](http://guides.rubyonrails.org/association_basics.html)
  + [Active Record Query Interface](Active Record Query Interface)

- Views
  + [Layouts and Rendering in Rails](http://guides.rubyonrails.org/layouts_and_rendering.html)
  + [Action View Form Helpers](http://guides.rubyonrails.org/form_helpers.html)

- Controllers
  + [Action Controller Overview](http://guides.rubyonrails.org/action_controller_overview.html)
  + [Rails Routing from the Outside In](http://guides.rubyonrails.org/routing.html)

- 其他必读
  + [Active Support Core Extensions](http://guides.rubyonrails.org/active_support_core_extensions.html)
  + [Securing Rails Applications](http://guides.rubyonrails.org/security.html)
  + [Rails Internationalization API](http://guides.rubyonrails.org/i18n.html)
  + [Rake Tasks](http://guides.rubyonrails.org/command_line.html)

#### 选读部分
  + [Action Mailer Basics](http://guides.rubyonrails.org/action_mailer_basics.html)
  + [Debugging Rails Applications](http://guides.rubyonrails.org/debugging_rails_applications.html)
  + [Configuring Rails Applications](http://guides.rubyonrails.org/configuring.html)
  + [Autoloading and Reloading Constants](http://guides.rubyonrails.org/autoloading_and_reloading_constants.html)
  + [Rails on Rack](http://guides.rubyonrails.org/rails_on_rack.html)
  + [Creating and Customizing Rails Generators](http://guides.rubyonrails.org/generators.html)
  + [Ruby on Rails Guides Guidelines](http://guides.rubyonrails.org/ruby_on_rails_guides_guidelines.html)

Rails源码
-----------
#### 查看源码工具
  * [Pry](https://hunzino1.github.io/other/2018/using_pry)

#### Rails源码模块
建议:
1. 先看activesupport, 再看activemodel, 最后看 activerecord, actionpack和actionview
2. 先看activesupport的core_ext/, 再看其他

  * [activesupport](https://hunzino1.github.io/other/2018/source_code_activesupport)
  * [activemodel](https://hunzino1.github.io/other/2018/source_code_activemodel)
  * [activerecord](https://hunzino1.github.io/other/2018/source_code_activerecord)
  * [actionpack](https://hunzino1.github.io/other/2018/source_code_actionpack)
  * [actionview](https://hunzino1.github.io/other/2018/source_code_actionview)

需要头脑风暴的知识点
-------------------------------
+ solr
+ nginx
+ memcached
+ C/S架构，从浏览器发出请求到渲染出页面发生了什么
+ passenger
+ cookie和session
+ rails安全
+ god
+ sidekiq


------------------------------------------------------------------------------------
提高篇

线程 源码
-----------
* Ruby under a Microscope
* Ruby with Threads
* Ruby with Tcp Sockets》
* Ruby with Unix Processes

服务间通信
---------
#### 接口
TODO 介绍这两种接口
* HTTP restful
> 介绍HTTP
> keywords

* thrift

#### 队列
TODO Keywords
* Redis
* Sidekiq

#### Kafka
  TODO Keywords
  * [基础知识和原理](http://www.infoq.com/cn/articles/kafka-analysis-part-1/)
  * [官方文档](http://kafka.apache.org/documentation.html)
  * Rails项目中使用kafka
  - [poseidon gem](https://github.com/bpot/poseidon)
  - [poseidon_cluster gem](https://github.com/bsm/poseidon_cluster)

#### 设计原则及实例
TODO

Web前端
-------
TODO Keywords, 学习链接

#### 浏览器：
* 调试工具(firebug, chrome)
  TODO 学习链接

  * 浏览器渲染原理
  TODO 学习链接

#### HTML
  TODO 学习链接

#### CSS
  TODO 学习链接

#### Javascript
  TODO 学习链接

#### Ajax
  TODO 学习链接

#### 提高篇
TODO 链接至前端知识树
* Web流行框架
* Node
* React
* AngularJs

数据库 @wanghao
------
#### MySQL 进阶
[初级](https://hunzino1.github.io/other/2018/mysql_basic_knowledge)
[中级](https://hunzino1.github.io/other/2018/mysql_secondary_knowledge)
[高级](https://hunzino1.github.io/other/2018/mysql_highest_knowledge)

#### TODO 添加数据库设计规范

#### 设计 @jingchaowei
* Sql Cookbook
TODO: 找电子版

#### 索引
高性能Mysql 第X章
TODO 学习链接

#### 慢查询优化
高性能Mysql 第X章

* TODO 优化思路

#### 引擎
  高性能Mysql 第X章

#### 主从结构
  高性能Mysql 第X章

网络安全 @dengqinghua 头脑风暴 sql注入 XSS CSRF 网络安全文档
--------
#### 学习资源
* Rails安全(TODO: Rails Security)
* 白帽子讲web安全(TODO: 添加书目标号)
* 黑客攻防技术宝典(TODO: 添加书目标号)

#### 常见安全问题
* 数据库注入
* XSS攻击
* TODO: 补充其他的攻击类型(可参考运维的安全培训文档)

设计原则
--------
#### Head first设计模式
(TODO: 添加书目标号 keywords)
#### ruby面向对象编程
(TODO: 添加书目标号 keywords)
#### 重构
(TODO: 添加书目标号 keywords)
#### 敏捷软件开发：原则、模式与实践
(TODO: 添加书目标号 keywords)

计算机基础
----------
#### 操作系统
  * 深入理解计算机系统
(TODO: 添加书目标号 keywords)

#### 算法结构
  * 编程珠玑
(TODO: 添加书目标号 keywords)
  * 框架核心算法
(TODO: 添加书目标号 keywords)

HTTP协议
--------
#### TCP/IP详解：卷一
(TODO: 添加书目标号 keywords)
#### 图解HTTP
(TODO: 添加书目标号 keywords)
#### 图解TCP/IP
(TODO: 添加书目标号 keywords)

缓存框架
--------
#### 浏览器端
(TODO keywords, resources)
#### CDN
(TODO keywords, resources)
#### varnish
(TODO keywords, resources)
#### nginx
(TODO keywords, resources)
#### Rails：object，partial，page，action
(TODO keywords, resources)
#### Memcache
(TODO keywords, resources)

异步处理框架
-----------
(TODO 入门文章, 优化一下目录)
#### 任务异步处理
  * Redis
  * Kafka

#### 页面异步处理
  * Ajax

搜索框架
-------
#### Solr
(TODO 入门文章)
#### ElasticSearch
(TODO 入门文章)

服务器框架
----------
(TODO 深入研究文章)
#### Nginx
(TODO 添加资源)

#### Passenger
  keywords
(TODO 官方文档)
(TODO ruby源码)

TODO: 和运维同学了解服务器端的技术栈

