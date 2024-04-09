---
title: "理解werkzeug中的Local对象"
date: 2019-07-18
draft: false
tags:
  - werkzeug
---
ThreadLocal是线程级别的local，如果在greenlet或者协程这种微线程环境下，或者在多个请求共用一个线程的情况下，线程级别是不够的。ThreadLocal是thread-safe和thread-specific的, 而有些情况需要greenlet-safe和greenlet-specific或者request-safe和request-specific。

werkzeug 0.1版中Local的实现是这样的：

```python
try:
    from py.magic import greenlet
    get_current_greenlet = greenlet.getcurrent
    del greenlet
except (RuntimeError, ImportError):
    get_current_greenlet = lambda: None
try:
    from thread import get_ident as get_current_thread
    from threading import Lock
except ImportError:
    from dummy_thread import get_ident as get_current_thread
    from dummy_threading import Lock
from werkzeug.utils import ClosingIterator


def get_ident():
    """
    Return a unique number for the current greenlet in the current thread.
    """
    return hash((get_current_thread(), get_current_greenlet()))


class Local(object):

    def __init__(self):
        self.__dict__.update(
            __storage={},
            __lock=Lock()
        )

    def __iter__(self):
        return self.__dict__['__storage'].iteritems()

    def __call__(self, proxy):
        """Create a proxy for a name."""
        return LocalProxy(self, proxy)

    def __getattr__(self, name):
        self.__dict__['__lock'].acquire()
        try:
            ident = get_ident()
            if ident not in self.__dict__['__storage']:
                raise AttributeError(name)
            try:
                return self.__dict__['__storage'][ident][name]
            except KeyError:
                raise AttributeError(name)
        finally:
            self.__dict__['__lock'].release()

    def __setattr__(self, name, value):
        self.__dict__['__lock'].acquire()
        try:
            ident = get_ident()
            storage = self.__dict__['__storage']
            if ident in storage:
                storage[ident][name] = value
            else:
                storage[ident] = {name: value}
        finally:
            self.__dict__['__lock'].release()

    def __delattr__(self, name):
        self.__dict__['__lock'].acquire()
        try:
            ident = get_ident()
            if ident not in self.__dict__['__storage']:
                raise AttributeError(name)
            try:
                del self.__dict__['__storage'][ident][name]
            except KeyError:
                raise AttributeError(name)
        finally:
            self.__dict__['__lock'].release()
```

可以看到，如果当前环境支持greenlet, 则get_ident是greenlet级别的ident(greentlet中有类似thread.current_thread的greenlet.getcurrent), 否则是thread级别。

测试过程：将上面的代码开头的`py.magic`改为`greenlet`, `__setattr__`中加入`print`, 保存为`local.py`。测试代码如下：

```python
import local
from greenlet import greenlet

lc = local.Local()

def test1():
	lc.test = 'test1'
	print("lc.test in test1: {}".format(lc.test))
	gr2.switch()
	print("lc.test in test1: {}".format(lc.test))
	print("test1 done.")

def test2():
	lc.test = 'test2'
	print("lc.test in test2: {}".format(lc.test))
	gr1.switch()
	print("test2 done.")

gr1 = greenlet(test1)
gr2 = greenlet(test2)
gr1.switch()
```

运行结果：

```shell
(werkzeug) localhost:iwerkzeug didi$ python test_local.py 
('ident: ', -3206197664300787518)
('storage: ', {-3206197664300787518: {'test': 'test1'}})
lc.test in test1: test1
('ident: ', -3206197296357035168)
('storage: ', {-3206197296357035168: {'test': 'test2'}, -3206197664300787518: {'test': 'test1'}})
lc.test in test2: test2
lc.test in test1: test1
test1 done.
```

可以看到，gr1和gr2中使用同样的参数`lc.test`，但却是greenlet specific的, 它得到的是自己的那份。原理其实也简单：将ident作为key，value中(也是一个dict)存放这个环境的一些变量。
