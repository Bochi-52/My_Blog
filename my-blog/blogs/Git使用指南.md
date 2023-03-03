---
title: Git使用指南
date: 2021-09-12
tags:
 - Git
categories:
 - 使用指南
---



# Git使用指南



## Git 是什么

>  Git 三个概念：
>
> - 提交 commit
> - 仓库 repository
> - 分支 branch

## 软件安装

> git	https://gitforwindows.org/
>
> vscode	https://code.visualstudio.com/
>
> vscode将git中许多命令简化了



## Git仓库初始化

```
$ git init
```



## 提交

``` 
配置个人信息
$ git config --global user.name "Yuor Name"
$ git config --global user.email "you@example.com"

$ git add -A	提交所有文件
$ git commit -m "提交信息"
```

<img src="https://pic.wangt.cc/download/pic_router.php?path=https://img-blog.csdnimg.cn/20201014001415560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNTExNDA1,size_16,color_FFFFFF,t_70#pic_center" style="zoom:40%;" />



## 查看提交历史

```
$ git log --stat
```



## 维护项目日常

```
工作区 放弃更改/撤销
$ git chekout <filename>
提交后撤回
$ git reset HEAD^
```



## 分支

```
在当前节点新建分支
$ git checkout -b <branchname>
例举所有的分支
$ git branch
单纯的切换到某个分支
$ git checkout <branchname>
删除特定的分支
$ git branch -D <branchname>
合并分支
$ git merge <branchname>
```



## Git与Github远程仓库

```
先设置远程仓库
GitHub有提示代码

推送 当前分支最新的提交到远程
$ git push
拉取 远程分支最新的提交到本地
$ git pull
```

















