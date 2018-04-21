---
layout: post
title: Your local changes to the following files would be overwritten by merge
categories: GitHub
description: Your local changes to the following files would be overwritten by merge
keywords: git,overwritten,stash
---

git提交提示error: Your local changes to the following files would be overwritten by merge。

1、保留本地修改的代码
```sh
git stash  
git pull origin master  
git stash pop 
```
2.只保留服务器代码

```sh
git reset --hard  
git pull origin master
```