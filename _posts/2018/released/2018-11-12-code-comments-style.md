---
layout:     post
title:      git和git-flow
category: rubytree
no-post-nav: true
tags: [rubytree]
excerpt: 每天都不忘保持一点点紧张，直到实现目标之后。
---

注释规范
========

该文档涵盖了文档注释的基本规范.

阅读完该文档后，您将会了解到:

* 注释的模板格式.
* 如何写注释.
* 写注释的注意事项.

--------------------------------------------------------------------------------

NOTE: 程序员最痛恨的两件事:
1. 别人的代码没有注释: WTF, 这行代码到底是什么意思?
2. 别人要求自己写注释: WTF, 我哪里有时间写注释?

NOTE: 写注释原则:
1. 沉住气;
2. 别急;
3. 慢慢写;
4. 写完后自己读一遍, 要能读通顺, 没有错别字, 意思要表达清楚.

注释的模板
----------
最重要的四个参数

- Description,    描述你的代码是做什么用的.
- Parameters,     代码所需要的参数是什么, 包含字段英文名和对应的中文说明, 数据类型(bool int string array hash)
- Usage/Examples, 举出调用的例子
- Returns,        列举出返回的内容的形式

INFO: 注释以`##`开始, 每一部分以空格分割, 注释的最后一行留空.

```ruby
class Role < ActiveRecord::Base
  ##
  # ==== Description
  #
  #     http://redmine.tuan800-inc.com/issues/50782
  #
  #   现在权限位均存至数据库，且不再使用位计算存储，而是用中间表的方式存储
  #
  #     +-----------------------------------------------------------+
  #     |             roles <-------+                               |
  #     |                           |                               |
  #     |                           |       role_admin_authorities  |
  #     |                           +----------+ role_id            |
  #     |                           +----------+ admin_authority_id |
  #     |                           |                               |
  #     | admin_authorities <-------+                               |
  #     +-----------------------------------------------------------+
  #
  #   其中admin_authorities为权限表，role_admin_authorities为权限角色中间表
  #   roles和role_admin_authorities是多对多的关系
  #
  has_many :role_admin_authorities
  has_many :authorities,
    source: :admin_authority, through: :role_admin_authorities

  ##
  # ==== Description
  #   判断当前角色是否可以执行某个操作
  #
  # ==== Parameters
  #   auth_key|[auth_key1, auth_key2, ..., auth_keyn]
  #
  # ==== Examples
  #   Role.first.can?(:view_1024)
  #   Role.first.can?([:view_1024, :rock_&_roll])
  #
  # ==== Returns
  #   true|false
  #
  def can?(keys)
    unless keys.is_a?(Array)
      keys = [keys]
    end

    keys.all? do |key|
      authorities.any? { |authority| authority.key == key.to_s }
    end
  end
end
```

Controller注释
--------------
Controller注释需要添加两点:

- Parameters 浏览器传入的参数, 这个非常重要, 需要写明每个字段的中文含义和类型
- URL        请求的method和地址, 如 POST /create_user
- Returns   如果是ajax请求, 则必须包含 status, msg 这两个字段, 其他字段需要给出对应的中文含义和类型.

status的可选值包括:

- 200 成功
- 500 程序异常

```ruby
##
# ==== Description
#   在ChargeController#prepay的action之后，Java端会根据服务器传的
# callback_url后发该请求.
#
#   需要注意的是, Java端发请求时，传入的参数仅为`订单编号: order_no`
# 根据该参数，可找到该订单对象, 订单的状态信息的更新，调用了
#
#    current_account.charge!(order)
#
# 方法，在Account#charge!方法中, 服务器又请求了一次Java端的支付接口，查询
# 该订单的状态.
#
# ==== Parameter
#   order_no: 订单编号
#
# ==== URL
#   GET /zhanghu/chongzhi/charge_callback
#
# ==== Returns
#   redirect_to|raise errors
#
def charge_callback
  if order = current_account.orders.with_serial_number(params[:order_no])
    if order.success?
      res = OpenStruct.new
      res.status = :success
    else
      res = current_account.charge(order)
    end

    redirect_to pay_zhanghu_chongzhi_result_url(
      status: res.status, no: params[:order_no]
    )
  else
    raise "订单不存在"
  end
end
```

逻辑设计注释
------------
对于复杂的架构设计, 需要在model或者class上面添加注释, 并描述一下这个架构是为了解决什么问题, 提供了哪些方法来使用

```ruby
##
# ==== Description
#  提供权限集合的数据结构接口, 支持嵌套的权限设置
#
# ==== Usage
#   该类提供了定义权限的方法，假如现在需要定义一套
# 帐号权限系统AccountAuthority, 则需继承Authority类，即
#
#   class AccountAuthority < Authority
#   end
#
#   和Rails中的routes设置类似，Authority类提供了
#
#      Authority.root       # 设置root权限, 仅能定义一次，必须存在root权限
#      Authority.namespace  # 设置层级权限, 该权限必须有下级的子权限和上级的父权限
#      Authority.set_auth   # 设置原子权限, 该权限必须有上级的父权限
#
#   三个private方法
#
#   设置权限的示例如下:
#
#     class AccountAuthority < Authority
#       root :account_manage, '账户管理', 2**0 do
#         namespace :order_manage, '订单管理', 2**1 do
#           set_auth :delete_order, '删除订单', 2**5
#           set_auth :update_order, '更新订单', 2**10
#         end
#
#         set_auth :refund_manage, '退款管理', 2**2
#       end
#     end
#
#   上述设置构建了权限树，如下图所示(level为树的深度, 即权限的级别)
#
#    level |
#     0    |               account_manage(:root, 2**0)
#          |                 /       \
#     1    |     order_manage(2**1) refund_manage(2**2)
#          |         /          \
#     2    | delete_order(2**5) update_order(2**10)
#
# ==== Main Methods
#   1. 查看已经定义的权限: AccountAuthority.defined_authorities
#
#     AccountAuthority.defined_authorities
#     #=>
#
#       {
#        :account_manage => {
#          :value         => "1",
#          :name          => "权限管理",
#          :parent_key    => nil,
#          :children_keys => [:order_manage],
#          :level         => 0
#        },
#        :order_manage   => {
#          :value         => "2",
#          :name          => "订单管理",
#          :parent_key    => :account_manage,
#          :children_keys => [:delete_order_manage, :update_order_manage]
#          :level         => 1
#        },
#        :delete_order => {
#          :value         => "512",
#          :name          => "删除",
#          :parent_key    => :order_manage,
#          :children_keys => [],
#          :level         => 2
#        },
#        :update_order => {
#          :value         => "1024",
#          :name          => "更新",
#          :parent_key    => :order_manage,
#          :children_keys => [],
#          :level         => 2
#        },
#        :refund_manage => {
#          :auth_key      => :refund_manage,
#          :value         => "4",
#          :name          => "退款管理",
#          :parent_key    => :account_manage,
#          :children_keys => [],
#          :level         => 1
#          }
#       }
#
#   由上可知，在 AccountAuthority.defined_authorities 中保存的key的形式有两种，分别为
#     - auth_key, 用户设置的权限的标识
#     - value,    权限位的值
#
#   可以通过auth_key取到对应的权限信息
#
#     AccountAuthority.defined_authorities[:order_manage]
#     #=>
#
#       {
#         :value         => "2",
#         :name          => "订单管理",
#         :parent_key    => :account_manage,
#         :children_keys => [:delete_order_manage, :update_order_manage],
#         :level         => 1
#       }
#
#   权限信息的keys有:
#     - value,         该权限位的值
#     - name,          该权限的中文含义
#     - parent_key,    该权限的父权限
#     - children_keys, 该权限的子权限
#     - level,         该权限的级别
#
#  2. 查找权限: AccountAuthority.find_by_auth_key(auth_key)
#    该方法定义在`AuthorityExt`中, 请参考AuthorityExt的注释文档.
#
#  3. 判断某个权限是否响应传入的权限位值: AccountAuthority#respond?(authority_value)
#    该方法定义在`AuthorityExt`中, 请参考AuthorityExt的注释文档.
#
#  4. 其他扩展的方法在AuthorityExt中，请查看AuthorityExt的注释
#
class Authority
  include ActiveModel::Model
  include AuthorityExt
  include Authority::Validations

  attr_accessor :name, :auth_key, :parent_key, :value, :children_keys, :level

  class << self
  end
end
```

Markdown格式注释
----------------
在罗列出多个参数的时候, 建议使用Markdown风格的结果

```ruby
##
# ==== Description
#   上传图片接口
#
# ==== Examples
#
#  uploader = ImageUploader.new(path: image_path, source: source, post_url: url)
#  uploader.upload!
#
#  - source:   JAVA图片上传服务器的源，需要JAVA提供该source的值,
#                默认为default = zhaoshang_identity_authentication,
#                default source支持1M以内, jpg, png和jpeg的图片.
#  - post_url: JAVA图片上传的地址.
#
#    * 如果上传图片成功，则Java成功返回了图片的key
#      uploader.valid?      # => true
#      uploader.errors.any? # => false
#      uploader.key         # => "1366x768.9a16ffbb48921063f3eb5b12b0c5c801.png"
#
#    * 如果上传图片失败, 则uploader的errors会存储错误信息
#      uploader.errors.any?          # => true
#      uploader.errors.full_messages # => ["错误信息"]
#
class ImageUploader
end
```

