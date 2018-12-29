---
layout:     post
title:      Active Record Callbacks
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

ActiveSupport中的callbacks
==========================
阅读本文,您将看到:

* ActiveRecord::Base#save的流程
* callbacks的定义与基本原理
* callbacks在rails3中的大致实现流程
* rails4和rails3在callbacks实现上的异同
* 日常使用中有哪些可能需要注意的细节


如果只是想了解ActiveRecord::Base#save时的callbacks调用流程,只看第一部分即可,之后是ActiveSupport::Callbacks的一些分析.


-------------------------------------------------------------------------

## ActiveRecord::Base.save的流程
### save的实现流程
在开始说``callbacks``之前,先介绍下平时调用`ActiveRecord::Base#save`的时候都经过了哪些`callbacks`流程:

```ruby
  def save
    vaild_result = \
      run_callbacks :validation do
        puts 'validate sth'
        true
      end

    if vaild_result
      ActiveRecord::Base.transaction do
        run_callbacks :save do
          create_or_update
        end
      end

      begin
        committed
      rescue Exception => ex
        Rails.logger.error(ex.inspect)
      end
      true
    else
      false
    end
  end

  def committed
    run_callbacks :commit do
      puts 'commit balabala...'
    end
  end

  def create_or_update_stub
    new_record? ? create : update
  end

  def create
    run_callbacks :create do
      puts 'INSERT INTO test_news balabala...'
    end
  end

  def update
    run_callbacks :update do
      puts 'update test_news balabala...'
    end
  end
```

### ActiveRecord::Callbacks的封装方式
`ActiveRecord::Callbacks`实际上是在`ActitveSupport::Callbacks`
的基础上封装了一些如`save`,`commit`,`validation`这样的回调,在我们调用如

```ruby
  after_commit :test
```
的时候,其实调用的是:

```ruby
  set_callback :commit, :after, :test
```
完成了`set_callback`的过程.


当然,在我们调用`after_commit`之前,首先会声明这个callbacks,例如

```ruby
define_callbacks :save
```
就定义了`:commit`这个`callback`.


而`run_callbacks`的方法则隐藏在对`save`,`commit`等方法的封装中.
### 调用save的时候callbacks的执行流程

 * (-) <tt>save</tt>
 * (-) <tt>valid</tt>
 * (1) <tt>before_validation</tt>
 * (-) <tt>validate</tt>
 * (2) <tt>after_validation</tt>
 * (3) <tt>before_save</tt>
 * (4) <tt>before_create</tt>
 * (-) <tt>create</tt>
 * (5) <tt>after_create</tt>
 * (6) <tt>after_save</tt>
 * (7) <tt>after_commit</tt>


其中(3)~(6)是在一个`transaction`里的.
整个回调链包含在一个事务中。如果任何一个 `before_*` 回调方法返回 `false` 或抛出异常，整个回调链都会终止执行，撤销事务；而 `after_*` 回调只有抛出异常才能达到相同的效果。
### 调用after_commit时抛出异常会怎么样
由于`after_commit`(以及`after_rollback`)并不在`save`的`transaction`中,所以如果`after_commit`触发了异常,
那么在**rails3**的`ActiveRecord::Transaction`中,有如下代码:

```ruby
# Call the after_commit callbacks
def committed! #:nodoc:
  run_callbacks :commit
ensure
  clear_transaction_record_state
end
```
从这里看异常是被抛出来了,然而调用`committed!`的地方:

```ruby
# Send a commit message to all records after they have been committed.
def commit_transaction_records
  records = @_current_transaction_records.flatten
  @_current_transaction_records.clear
  unless records.blank?
    records.uniq.each do |record|
      begin
        record.committed!
      rescue Exception => e
        record.logger.error(e) if record.respond_to?(:logger) && record.logger
      end
    end
  end
end
```
**直接捕获了并没有再抛出...**


这样就导致`commit`的`CallbackChain`中异常后的其他的`Callback`都没有执行,并且除了系统日志外基本没有什么痕迹留下来,出了bug基本没法查.


正在考虑在这个地方加一个日志,将`after_commit`的异常存起来,以供bug查询用.


## callbacks的定义与基本原理

### 定义
在rails的源码注释中,对于**callbacks**的基本定义是**在对象的生命周期中的某些关键点执行的一些钩子代码**.
### 基本原理
如果我们要定义并一个**callbacks**,基本步骤有三个,分别对应三个**ActiveSupport::Callbacks**中的三个基本方法:
* **define_callbacks**
这个方法的主要作用是定义一个**callbacks**

  我们来看一个只include了`ActiveSupport::Callbacks`的例子:

 ```ruby

class Record
  include ActiveSupport::Callbacks
  define_callbacks :update_order
  def update_order
      run_callbacks :update_order do
          puts "- update_order"
      end
  end
end
```

在上面的代码中我们定义了一个叫做:**update_order**的**callbacks**,然后我们又发现在**update_order**这个方法中,调用了**run_callbacks :update_order**,这个方法的作用就是用来执行刚才定义过的:**update_order**的**callbacks**.
但是在执行该**callbacks**的时候,我们发现我们只是定义了这个**callbacks**,并没有告诉程序在这个**callbacks**中我们要执行什么,于是就有了下面这个方法:

```ruby
set_callback :update_order, :before, :test1
```
完整代码应该是这样子:

```ruby
class Record
  include ActiveSupport::Callbacks
  define_callbacks :update_order
  set_callback :update_order, :before, :test1

  def update_order
      run_callbacks :update_order do
          puts "- update_order"
      end
  end

  def test1
    puts 'this is a callback.'
  end
end
```
**set_callback**这个方法将我们定义的需要在:**update_order**中执行的代码块传入了这个**callbacks**中,现在执行的结果如下:

```ruby
a = Record.new
<Record:0x007ff7c0615d18>
a.update_order
this is a callback.
- update_order
```
我们看到,一个**callbacks**需要首先经过**声明定义**(`define_callbacks`),然后**设置**(`set_callback`),最后在需要的地方**执行**(`run_callbacks`),这样才是一个完整有意义的`callbacks`.
## callbacks在rails3中的大致实现流程
### define_callbacks
源码如下:

```ruby
def define_callbacks(*callbacks)
  config = callbacks.last.is_a?(Hash) ? callbacks.pop : {}
  callbacks.each do |callback|
    class_attribute "_#{callback}_callbacks"
    send("_#{callback}_callbacks=", CallbackChain.new(callback, config))
    __define_runner(callback)
  end
end
```
  *callbacks这个参数里是需要声明的callbacks的名字,例如:update_order,然后给每个callbacks声明了set,get和runner(执行方法).
  这里可以看到`_update_order_callbacks`本身是一个`CallbackChain`,这是一个队列,存放了每个需要执行的代码块的信息.
  这里要说明的是` __define_runner `这个方法,源码如下:

  ```ruby
# Generate the internal runner method called by +run_callbacks+.
def __define_runner(symbol) #:nodoc:
  runner_method = "_run_#{symbol}_callbacks"
  unless private_method_defined?(runner_method)
    class_eval <<-RUBY_EVAL, __FILE__, __LINE__ + 1
      def #{runner_method}(key = nil, &blk)
        self.class.__run_callback(key, :#{symbol}, self, &blk)
      end
      private :#{runner_method}
    RUBY_EVAL
  end
end
   ```
   这个方法定义了一个叫做`_run_update_order_callbacks`的方法,run_callbacks执行的时候就是在调用这个方法,到这里,`define_callbacks`的工作就做完了.
### set_callback
定义了`_update_order_callbacks`这个`CallbackChain`之后,这个队列是空的,如果不掉用`set_callback`方法加入`callbacks`需要执行的代码块的信息,那么运行`runner_callbacks`的时候就相当于没有执行任何有效代码.

```ruby
def set_callback(name, *filter_list, &block)
  mapped = nil

  __update_callbacks(name, filter_list, block) do |target, chain, type, filters, options|
    mapped ||= filters.map do |filter|
      Callback.new(chain, filter, type, options.dup, self)
    end

    filters.each do |filter|
      chain.delete_if {|c| c.matches?(type, filter) }
    end

    options[:prepend] ? chain.unshift(*(mapped.reverse)) : chain.push(*mapped)

    target.send("_#{name}_callbacks=", chain)
  end
end
   ```
### run_callbacks
  这个方法是执行`callbacks`的主要方法,但是主要实现并不在该方法中,该方法源码如下:

```ruby
def run_callbacks(kind, *args, &block)
  send("_run_#{kind}_callbacks", *args, &block)
end
```
其中`_run_#{kind}_callbacks`这个方法之前在调用`define_callbacks`时已经声明过了,是这个样子:

```ruby
def _run_#{symbol}_callbacks(key = nil, &blk)
  self.class.__run_callback(key, :#{symbol}, self, &blk)
end
private :#{_run_#{symbol}_callbacks}
```

其中有个类方法`__run_callback`,这个方法才是执行时的主要方法:

```ruby
# This method calls the callback method for the given key.
# If this called first time it creates a new callback method for the key,
# calculating which callbacks can be omitted because of per_key conditions.
#
def __run_callback(key, kind, object, &blk) #:nodoc:
  name = __callback_runner_name(key, kind)
  unless object.respond_to?(name, true)
    str = object.send("_#{kind}_callbacks").compile(key, object)
    class_eval <<-RUBY_EVAL, __FILE__, __LINE__ + 1
      def #{name}() #{str} end
      protected :#{name}
    RUBY_EVAL
  end
  object.send(name, &blk)
end
```
  其中`compile`方法的作用是根据`CallbackChain`中的各个`callback`生成一个可执行的完整的包含原方法逻辑的方法(的string),然后通过send来调用这个生成的方法,整个回调的执行过程就完成了,`compile`的源码:

  ```ruby
 def compile(key=nil, object=nil)
   method = []
   method << "value = nil"
   method << "halted = false"

   each do |callback|
     method << callback.start(key, object)
   end

   if config[:rescuable]
     method << "rescued_error = nil"
     method << "begin"
   end

   method << "value = yield if block_given? && !halted"

   if config[:rescuable]
     method << "rescue Exception => e"
     method << "rescued_error = e"
     method << "end"
   end

   reverse_each do |callback|
     method << callback.end(key, object)
   end

   method << "raise rescued_error if rescued_error" if config[:rescuable]
   method << "halted ? false : (block_given? ? value : true)"
   method.compact.join("\n")
 end
  ```
  其中`start`和`end`分别是对`:before`和`:after`的``callback``解析,而`:around`则在前后分别都有解析.
## rails4和rails3在callbacks实现上的异同
**rail4**和**rails3**的这块的总体实现思路是一样的,都是将需要实现回调的代码块传入到run_callbacks方法中,然后插入到声明过的`callbacks`中间执行,但是**rails4**用了更好的封装来实现,并且从设计上改进了一些问题,比如说加了线程锁,每次调用`run_callbacks`时实质上都是根据判断执行相应的代码块,避免了用`eval`这种方式拼接方法带来的可能出现的问题.
 主要实现方式不同的地方有:

 ```ruby
 def __run_callbacks__(callbacks, &block)
   if callbacks.empty?
     yield if block_given?
   else
     runner = callbacks.compile
     e = Filters::Environment.new(self, false, nil, block)
     runner.call(e).value
   end
 end
 ```
 其中的`runner`是在`compile`方法中得到了一个`CallbackSequence`的对象,这个对象有`@before`和`@after`两个实例变量,都是`Array`,里面存放了需要执行的很多`proc`,调用call方法的时候会先调用`@before`里的每个`proc`,再执行方法本身的代码块,再执行`@after`中的`proc`,完成整个回调.

 ```ruby
 def compile
   @callbacks || @mutex.synchronize do
     final_sequence = CallbackSequence.new { |env| Filters::ENDING.call(env) }
     @callbacks ||= @chain.reverse.inject(final_sequence) do |callback_sequence, callback|
       callback.apply callback_sequence
     end
   end
 end
```
其中的`apply`方法:

```ruby
def apply(callback_sequence)
  user_conditions = conditions_lambdas
  user_callback = make_lambda @filter

  case kind
  when :before
    Filters::Before.build(callback_sequence, user_callback, user_conditions, chain_config, @filter)
  when :after
    Filters::After.build(callback_sequence, user_callback, user_conditions, chain_config)
  when :around
    Filters::Around.build(callback_sequence, user_callback, user_conditions, chain_config)
  end
end
  ```
这里的`user_coditions`是根据传入参数生成的判断是否执行的代码块,而`make_lambda`则是根据传入的`filter`类型的不同生成了相应的执行代码块,最后通过相应的`build`方法生成真正执行的代码块,最后放到传入的`callback_sequence`中.
以上列举了**rails4**代码和**rails3**代码的一些区别,而最新的**rails v5.0.0.1**在这块的的实现与**rails4**只有一些细节上的修改,但是在**rails5**的最后发现了一段warning:

```
Returning `false` in Active Record and Active Model callbacks will not implicitly halt a callback chain in Rails 5.1.
To explicitly halt the callback chain, please use `throw :abort` instead.
```
所以5.1看起来会很大程度修改这一块的condition判断条件.
