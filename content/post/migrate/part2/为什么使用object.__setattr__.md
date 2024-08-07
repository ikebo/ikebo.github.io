---
title: "为什么使用object.__setattr__"
date: 2019-07-23
draft: false
tags:
  - werkzeug
---
werkzeug 0.6.1中Local的初始化是这样的：

```python
class Local(object):
    __slots__ = ('__storage__', '__lock__')

    def __init__(self):
        object.__setattr__(self, '__storage__', {})
        object.__setattr__(self, '__lock__', allocate_lock())
```

我当时很奇怪为什么要用`object.__setattr__`, 而不是直接用`self.__storage__`, 当我直接用`self.__storage__ = {}`实现的时候才发现问题：

`self.__storage__`会调用`__setattr__`，而`__setattr__`中会调用`self.__lock__.acquire()`，因为此时`self.__lock__`还没有定义, 所以会调用`self.__getattr__`，而`self.__getattr__`中也会调用`self.__lock__.acquire()`, 此后就会一直调用`self.__getattr__`，最终导致StackOverflow。

而显示调用`object.__setattr__`就不会触发Local内部的`__setattr__`，从而避免上述情况。而且两者的效果是一样的，`object.__setattr__`的第一个参数是`self`，也就是这个实例，所以并不用担心是不是在父类定义了一个公共属性。

类似的还有使用`object.__getattribute__`的情况，一般也是为了避免无限递归。
