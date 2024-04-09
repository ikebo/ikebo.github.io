---
title: "git 放弃本地修改"
date: 2019-08-09
draft: false
tags:
  - git
---
Look[^first]:

```shell
git checkout . #本地所有修改的。没有的提交的，都返回到原来的状态
git stash #把所有没有提交的修改暂存到stash里面。可用git stash pop回复。
git reset --hard HASH #返回到某个节点，不保留修改。
git reset --soft HASH #返回到某个节点。保留修改
git reset HEAD^  #撤销上一次commit，回到没有add的状态

git clean -df #返回到某个节点
git clean 参数
    -n 显示 将要 删除的 文件 和  目录
    -f 删除 文件
    -df 删除 文件 和 目录
```

也可以使用：

```shell
git checkout . && git clean -xdf
```


[^first]: [CSDN](https://blog.csdn.net/leedaning/article/details/51304690)
