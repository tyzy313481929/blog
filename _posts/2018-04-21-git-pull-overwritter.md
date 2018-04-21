---
layout: post
title: git提交提示error: Your local changes to the following files would be overwritten by merge
categories: PHP
description:  git提交提示error: Your local changes to the following files would be overwritten by merge
keywords: git,overwritten,stash
---

git提交提示error: Your local changes to the following files would be overwritten by merge。

1、保留本地修改的代码

```shell

git stash  
git pull origin master  
git stash pop 

2.只保留服务器代码

```shell
git reset --hard  
git pull origin master
