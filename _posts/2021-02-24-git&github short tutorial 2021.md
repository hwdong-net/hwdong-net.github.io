---
layout:       post
title:        "Git基础:远程仓库"
subtitle:     "git&github short tutorial 2021"
date:         2021-02-24 16:16:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - IT
---

**git clone** 命令可将一个远程repo 克隆（clone）到本地文件夹并创建一个本地的repository，在本地的ticgit仓库中，为远程的repo创建一个默认名origin的关联。
今后可以用过origin名字将修改push到远程repo或将远程repo内容拉到(pull)本地。
```bash
$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.
```
用**git remote**命令查看本地repo有多少个短名（如origin）关联了远程的Repo，可以通过短名字去读（fetch/push）写远程的repo.
```bach
$ cd ticgit
$ git remote
origin
```

可以通过选项参数"-v"显示本地git的远程repo的关联短名对应的远程Repo的URL地址，
```bach
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
```

**git remote add**可以添加其他远程Repo并为这些远程repo指定本地的短名字。如下面命令先查询当前有多少个远程的repo，然后添加一个新的名为pb的远程Repo：
```bash
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
pb	https://github.com/paulboone/ticgit (fetch)
pb	https://github.com/paulboone/ticgit (push)
```
你可以通过**fetch**命令将pb的远程Repo内容拉取到本地来：
```bash
$ git fetch pb
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 43 (delta 10), reused 31 (delta 5)
Unpacking objects: 100% (43/43), done.
From https://github.com/paulboone/ticgit
 * [new branch]      master     -> pb/master
 * [new branch]      ticgit     -> pb/ticgit
```
这个fetch命令获取了pb名关联的远程存储的2个分支pb/master和pb/ticgit。

用push命令可将本地修改的内容push到远程Repo上，如：
```bash
$ git push origin master
```
将本地短名origin的修改内容push到它对应的那个远程Repo（即https://github.com/schacon/ticgit）的master分支上。

**git remote show**可以查询一个短名关联的远程Repo的更多信息：
```bash
$ git push origin master
* remote origin
  URL: https://github.com/my-org/complex-project
  Fetch URL: https://github.com/my-org/complex-project
  Push  URL: https://github.com/my-org/complex-project
  HEAD branch: master
  Remote branches:
    master                           tracked
    dev-branch                       tracked
    markdown-strip                   tracked
    issue-43                         new (next fetch will store in remotes/origin)
    issue-45                         new (next fetch will store in remotes/origin)
    refs/remotes/origin/issue-11     stale (use 'git remote prune' to remove)
  Local branches configured for 'git pull':
    dev-branch merges with remote dev-branch
    master     merges with remote master
  Local refs configured for 'git push':
    dev-branch                     pushes to dev-branch                     (up to date)
    markdown-strip                 pushes to markdown-strip                 (up to date)
    master                         pushes to master                         (up to date)
```

[Git Basics - Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)
