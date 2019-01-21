---
layout:     post
title:      Git常用命令
no-post-nav: true
category: rubytree
tags: [rubytree]
excerpt: 求真务实
---

Git常用命令
===========

该文档标注了使用Git必须掌握的命令.

阅读完该文档后，您将会了解到:

* 什么是Git.
* Git的基本命令.

--------------------------------------------------------------------------------

什么是Git
----------
> 分散式版本控制软件
> Git (/ɡɪt/) is a distributed revision control system with an emphasis on speed data integrity, and support for distributed, non-linear workflows

请查看: [什么是版本控制?](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)

Git常用命令
----------

```bash
git --help

usage: git [--version] [--help] [-C <path>] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

The most commonly used git commands are:
   add        Add file contents to the index
   bisect     Find by binary search the change that introduced a bug
   branch     List, create, or delete branches
   checkout   Checkout a branch or paths to the working tree
   clone      Clone a repository into a new directory
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   fetch      Download objects and refs from another repository
   grep       Print lines matching a pattern
   init       Create an empty Git repository or reinitialize an existing one
   log        Show commit logs
   merge      Join two or more development histories together
   mv         Move or rename a file, a directory, or a symlink
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects
   rebase     Forward-port local commits to the updated upstream head
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index
   show       Show various types of objects
   status     Show the working tree status
   tag        Create, list, delete or verify a tag object signed with GPG

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
```

### 常用命令列表

- git clone
- git status
- git checkout
- git branch -a|-r
- git add
- git commit
- git rm
- git remote update -p
- git push origin XXX
- git push origin :XXX
- git pull origin XXX
- git merge
- git log
- git blame
- git diff XXX
- git stash
- git push origin --delete
- git gc
- git init
- git branch -m 'name'

可以通过`git [command] --help` 来查看每一个命令的含义，如查看`git checkout`, 只需要在控制台中输入

```bash
git checkout --help
```

即可

请查看: [Git命令文档](http://gitref.org/)

### Git目录结构和配置
#### 配置
- git config --global user.name
- git config --global user.email

#### 目录结构
.git 每个文件是做什么用的
.git/config 文件每一行的意思
.gitignore

[配置介绍](https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE)
