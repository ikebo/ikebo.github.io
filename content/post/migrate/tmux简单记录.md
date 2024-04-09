---
title: "tmux简单记录"
date: 2019-07-02
draft: false
tags:
  - tmux
---
Session

新建session

```
tmux new -s sessionname
```

退出当前session

```
prefix + d
```

进入session

```
tmux at -t sessionname
```

关闭session

```
tmux kill-session -t sessionname
```

列出所有session

```
tmux ls
```

## Window

新建window

```
prefix + c
```

切换window:

```
prefix + p/n
```

列出所有window:

```
prefix + w
```

删除当前window:

```
prefix + &
```

## Pane

切分pane

```
prefix + "/%
```

切换pane:

```
prefix + o
```

删除当前pane:

```
prefix + x
```

重启pane:

```
prefix + :
```

输入 `respawn-pane -k`，然后 `Enter`

> prefix 默认为ctrl + b, 可自定义
