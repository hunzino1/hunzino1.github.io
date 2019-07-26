---
layout:     post
title:      基础必知(001) - mac电脑环境安装
category: base
tags: [base]
excerpt: 一个人有多努力，就会有多幸运。
---

基础必知(001) - mac电脑环境安装
=======================================

2019年07月1号，是我入职新东家的第一天，第一天嘛

总是安装环境

但是之前从未想过，所以在此总结一把，主要包括：

1. vim
2. git
3. idea
4. mysql

-----------------------------------------

1 vim
----------------------------------------

虽然之前不确定，但是现在确定，只要我还在编程，估计就离不开vim了

vim很简单，直接下载iterm2安装包即可。

但是样式快捷键的配置是个问题。

[样式传送门](https://github.com/hunzino1/dotfiles)

在此感谢老送，带我入门带我飞

傻瓜式配置

```shell
git clone https://github.com/dengqinghua/dotfiles.git ~/Downloads/dotfiles
cp ~/Downloads/dotfiles/vim/vimrc ~/.vimrc
vim +PluginInstall +qall
```

可能会有如图报错，直接回车，静静等待安装完成即可

![vimplugin](https://hunzino1.github.io/assets/images/2019/base/vimplugin.png)

2 git
---------------------------------------

安装git之前必须先安装homebrew，但是国外网站太慢

所以可以参考

[HomeBrew安装很慢](https://blog.csdn.net/qq_33591200/article/details/82882562)

[git安装](http://sourabhbajaj.com/mac-setup/)

3 idea
------------------------------------

傻瓜式安装之后，搭配vim配置

[样式传送门](https://github.com/hunzino1/dotfiles)

```shell
git clone https://github.com/dengqinghua/dotfiles.git ~/Downloads/dotfiles
cp ~/Downloads/dotfiles/vim/ideavimrc ~/.ideavimrc

```

4 mysql
-----------------------------------

略
