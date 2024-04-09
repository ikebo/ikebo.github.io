---
title: "ThreadLocal"
date: 2019-07-17
draft: false
tags:
  - thread local
---
TheadLocal 用于多线程环境下，线程之间可以使用相同的变量，而这个变量只与当前线程环境有关。[`werkzeug`](https://werkzeug.palletsprojects.com/en/0.15.x/local/)中有类似的实现，使每个路由处理函数都可使用相同的`request`变量，而这个对象的内容只与当前请求有关。

例如:[^first]

```python
import threading
  
# 创建全局ThreadLocal对象:
local_school = threading.local()

def process_student():
    # 获取当前线程关联的student:
    std = local_school.student
    print('Hello, %s (in %s)' % (std, threading.current_thread().name))

def process_thread(name):
    # 绑定ThreadLocal的student:
    local_school.student = name
    process_student()

t1 = threading.Thread(target= process_thread, args=('Alice',), name='Thread-A')
t2 = threading.Thread(target= process_thread, args=('Bob',), name='Thread-B')
t1.start()
t2.start()
t1.join()
t2.join()
```

执行结果：

```shell
Hello, Alice (in Thread-A)
Hello, Bob (in Thread-B)
```


[^first]: [廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/1016959663602400/1017630786314240)
