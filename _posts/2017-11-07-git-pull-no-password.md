---
layout: post
title: linux服务器git pull/push时提示输入账号密码之免除设置
categories: GitHub
description: linux服务器git pull/push时提示输入账号密码之免除设置
keywords: git,GitHub
---

1、先cd到根目录，执行git config --global credential.helper store命令
```sh
[root@iZ25mi9h7ayZ ~]# git config --global credential.helper store
```
2、执行之后会在.gitconfig文件中多加红色字体项
```sh
[user]
        name = xx
        email = xxxx@xxxx.com
[credential]
        helper = store
```
3、之后cd到项目目录，执行git pull命令，会提示输入账号密码。输完这一次以后就不再需要，并且会在根目录生成一个.git-credentials文件
```sh
[root@iZ25mi9h7ayZ test]# git pull
Username for 'https://git.oschina.net': xxxx@xxxx.com
Password for 'https://xxxx@xxxx.com@git.oschina.net':
[root@iZ25mi9h7ayZ ~]# cat .git-credentials
https://Username:Password@git.oschina.net
```
4、之后pull/push代码都不再需要输入账号密码了~
## 参考

[1]: http://www.cnblogs.com/orzlin/p/5613310.html
