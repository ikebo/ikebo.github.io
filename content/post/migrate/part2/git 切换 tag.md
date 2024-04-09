---
title: "git 切换 tag"
date: 2019-07-12
draft: false
tags:
  - git
---
有一种源码学习的方法是这样的：从最初的版本开始看，有的很大的开源项目最初可能就只有几百行。我们可以先把项目clone到本地，然后切换到最初般。

列出所有版本:

```bash
git tag
```

若一个tag都没有，则可能是因为你先fork了这个项目，然后本地再pull下来的，这种情况得先执行:

```bash
git fetch
```

然后，切换到指定版本：

```bash
git checkout tagname
```

切回到主分支：

```bash
git checkout master
```
