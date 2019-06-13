---
layout:     post
title:      创建一个spring(Maven)项目
category: spring
tags: [spring]
excerpt: 一个人有多努力，就会有多幸运。
---

创建一个spring(Maven)项目
=======================================

为什么写这个博客？

1. 从基础做起，不错过一点一滴
2. 中间真的遇到了很多意想不到的事
3. 之前上手就做，基础还是不展示，摇摇欲坠（借口）
4. 真正从0做一个项目，熟悉各部分的连接原理，比如POM的依赖是怎么加载到spring中的。

所以，come on

-----------------------------------------

1 新建一个spring项目
------------------------------------------

[github demo](https://github.com/hunzino1/spring_round_one/tree/master/muke/chapter2)

### 1.1 新建一个spring项目

> 其实没啥子可说的，出问题和疑惑都不在这

1 在首先点击Create New Project

![1](https://hunzino1.github.io/assets/images/2019/spring/new_spring/1.jpg)

2 勾选Spring然后next

![2](https://hunzino1.github.io/assets/images/2019/spring/new_spring/2.jpg)

3 设置你项目所想要存放的路径以及名字

> 注：IntelliJ IDEA中的project相当于eclipse中的workspace，而module才相当于一个project，
>
> 所以我们不需要创建workspace，IntelliJ IDEA默认也会给你创建一个module。

![3](https://hunzino1.github.io/assets/images/2019/spring/new_spring/3.jpg)

4 这时候IntelliJ IDEA就会自动下载Spring所需要的jars，只需要等待就好。

![4](https://hunzino1.github.io/assets/images/2019/spring/new_spring/4.jpg)

5 下载好后，Spring的jars和配置文件都准备好了。 

![5](https://hunzino1.github.io/assets/images/2019/spring/new_spring/5.jpg)

### 1.2 跑起来之前。。。

上面建立一个空的spring项目没任何问题，而在code时短短几行代码运到好多问题

#### 1.2.1 包名不能叫interface？

这个可能是自己第一次spring空项目不合规造成的，不过想build、interface等的确有人

遇到过类似问题，所以像这种类似关键字的尽量避免。

#### 1.2.2 Module Setting

1 首先Module Setting在哪打开？

如图，只能右键项目名，其他目录不行

![module_setting](https://hunzino1.github.io/assets/images/2019/spring/new_spring/module_setting.png)

2 Module Setting的作用？

正确的Module Setting如图

![module_setting2](https://hunzino1.github.io/assets/images/2019/spring/new_spring/module_setting2.png)

```html
1. 目录相对位置固定
    idea spring的目录结构是固定的，main -- java -- com
2. java 设置sources属性
    试了com、main设置属性，xml均报错
    如下图，将main设置sources属性之后，spring目录结构发生了变化
    xml也报错了

[TODO]所以module setting还是需要研究其原理。
```

![module_setting3](https://hunzino1.github.io/assets/images/2019/spring/new_spring/module_setting3.png)

2 创建一个maven项目
-------------------------------------------

待实践
