---
title: "Python 获取线程中的异常信息"
date: 2019-09-12
draft: false
tags:
  - Python
---
通常情况下我们无法将多线程中的异常带回主线程，所以也就无法打印线程中的异常，而通过traceback模块，我们可以对线程做如下修改，从而实现捕获线程异常的目的[^first]。

```python
import threading
import traceback

def my_func():
    raise BaseException("thread exception")


class ExceptionThread(threading.Thread):

    def __init__(self, group=None, target=None, name=None, args=(), kwargs=None, verbose=None):
        """
        Redirect exceptions of thread to an exception handler.
        """
        threading.Thread.__init__(self, group, target, name, args, kwargs, verbose)
        if kwargs is None:
            kwargs = {}
        self._target = target
        self._args = args
        self._kwargs = kwargs
        self._exc = None

    def run(self):
        try:
            if self._target:
                self._target()
        except BaseException as e:
            import sys
            self._exc = sys.exc_info()
        finally:
            #Avoid a refcycle if the thread is running a function with
            #an argument that has a member that points to the thread.
            del self._target, self._args, self._kwargs

    def join(self):
        threading.Thread.join(self)
        if self._exc:
            msg = "Thread '%s' threw an exception: %s" % (self.getName(), self._exc[1])
            new_exc = Exception(msg)
            raise new_exc.__class__, new_exc, self._exc[2]


t = ExceptionThread(target=my_func, name='my_thread')
t.start()
try:
    t.join()
except:
    traceback.print_exc()
```

输出如下:

```python
Traceback (most recent call last):
  File "/data/code/testcode/thread_exc.py", line 43, in <module>
    t.join()
  File "/data/code/testcode/thread_exc.py", line 23, in run
    self._target()
  File "/data/code/testcode/thread_exc.py", line 5, in my_func
    raise BaseException("thread exception")
Exception: Thread 'my_thread' threw an exception: thread exception
```

这样我们就得到了线程中的异常信息。


[^first]: [简书](https://www.jianshu.com/p/a8cb5375171a)
