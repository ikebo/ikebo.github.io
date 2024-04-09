---
title: "Python 限制函数运行时间"
date: 2019-09-11
draft: false
tags:
  - Python
  - signal
---
实际项目中会涉及到需要对有些函数的响应时间做一些限制，如果超时就退出函数的执行，停止等待。

## 使用signal

使用signal有所限制，需要在linux系统上，并且需要在主线程中使用。方法二使用线程计时，不受此限制。

```python
# coding=utf-8
import signal
import time
 
def set_timeout(num, callback):
    def wrap(func):
        def handle(signum, frame):  # 收到信号 SIGALRM 后的回调函数，第一个参数是信号的数字，第二个参数是the interrupted stack frame.
            raise RuntimeError
 
        def to_do(*args, **kwargs):
            try:
                signal.signal(signal.SIGALRM, handle)  # 设置信号和回调函数
                signal.alarm(num)  # 设置 num 秒的闹钟
                print('start alarm signal.')
                r = func(*args, **kwargs)
                print('close alarm signal.')
                signal.alarm(0)  # 关闭闹钟
                return r
            except RuntimeError as e:
                callback()
 
        return to_do
 
    return wrap
 
def after_timeout():  # 超时后的处理函数
    print("Time out!")
 
@set_timeout(2, after_timeout)  # 限时 2 秒超时
def connect():  # 要执行的函数
    time.sleep(3)  # 函数执行时间，写大于2的值，可测试超时
    print('Finished without timeout.')
 
if __name__ == '__main__':
    connect()
```

## 使用Thread

Thread方法在linux和windows环境下都可以使用，不过适用于python3.3及以下版本，从python3.4版本开始Thread部分函数有更改。

```python
# -*- coding: utf-8 -*-
from threading import Thread
import time
 
class TimeoutException(Exception):
    pass
 
ThreadStop = Thread._Thread__stop
 
def timelimited(timeout):
    def decorator(function):
        def decorator2(*args,**kwargs):
            class TimeLimited(Thread):
                def __init__(self,_error= None,):
                    Thread.__init__(self)
                    self._error =  _error
 
                def run(self):
                    try:
                        self.result = function(*args,**kwargs)
                    except Exception,e:
                        self._error = str(e)
 
                def _stop(self):
                    if self.isAlive():
                        ThreadStop(self)
 
            t = TimeLimited()
            t.start()
            t.join(timeout)
 
            if isinstance(t._error,TimeoutException):
                t._stop()
                raise TimeoutException('timeout for %s' % (repr(function)))
 
            if t.isAlive():
                t._stop()
                raise TimeoutException('timeout for %s' % (repr(function)))
 
            if t._error is None:
                return t.result
 
        return decorator2
    return decorator
 
@timelimited(2)  # 设置运行超时时间2S
def fn_1(secs):
    time.sleep(secs)
    return 'Finished without timeout'
 
def do_something_after_timeout():
    print('Time out!')
 
if __name__ == "__main__":
    try:
        print(fn_1(3))  # 设置函数执行3S
    except TimeoutException as e:
        print(str(e))
        do_something_after_timeout()
```

## 使用eventlet

eventlet在python3下可用。[了解eventlet](http://luckylau.tech/2017/03/06/Python%E7%9A%84eventlet%E4%BD%BF%E7%94%A8%E4%B8%8E%E7%90%86%E8%A7%A3/)

```python
import requests
import eventlet
import time
 
eventlet.monkey_patch()
 
time_limit = 3  #set timeout time 3s
 
with eventlet.Timeout(time_limit,False):
    time.sleep(5)
    r=requests.get("https://me.csdn.net/dcrmg", verify=False)
    print('error')
print('over')
```

转载自[CSDN](https://blog.csdn.net/dcrmg/article/details/82850457)
