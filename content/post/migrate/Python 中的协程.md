---
title: "Python 中的协程"
date: 2019-07-05
draft: false
tags:
  - Python
  - 协程
---
# Python 中的协程

函数也叫子程序，其调用过程一般为：Main中调用A，等待A结束后调用B，等待B结束后调用C... 函数的调用一般是单入口。

![20x25](/uploads/2021/04/2240683489.png)

相比于函数，协程可以有多个入口来暂停(切换到其他协程执行)和恢复协程的执行。另外，协程的调用不像函数调用需要主函数按照特定顺序依次调用子程序，协程之间是协作关系，可以来回切换。

![30x20](/uploads/2021/04/2476237448.png)

相比于线程，他们都是通过切换达到协作的目的。线程是由操作系统调度来实现切换，而协程是语言级别的切换，开销更小。

Python中，可以用生成器中的`yield`实现协程（支持不完全）

## 协程实现生产者／消费者模型[^first]

```python
import time

def consumer():
	r = ''
	while True:
		n = yield r
		if not n:
			return
		print('consuming {}'.format(n))
		time.sleep(1)
		r = '200 OK'

def produce(c):
	next(c)  # 启动协程，Python2写法： c.next()
	n = 0
	while n < 5:
		n = n + 1
		print('producing: {}'.format(n))
		r = c.send(n)
		print('consumer return: {}'.format(r))
	c.close() # 关闭协程

c = consumer()
produce(c)
```

运行结果：

```python
producing: 1
consuming 1
consumer return: 200 OK
producing: 2
consuming 2
consumer return: 200 OK
producing: 3
consuming 3
consumer return: 200 OK
producing: 4
consuming 4
consumer return: 200 OK
producing: 5
consuming 5
consumer return: 200 OK
```

## 用连接协程的方式创建管道[^second]

![](/uploads/2021/04/1886392579.png)

```python
def producer(sentence, next_coroutine):
    tokens = sentence.split(' ')
    for token in tokens:
        next_coroutine.send(token)
    next_coroutine.close()


def pattern_filter(pattern='ing', next_coroutine=None):
    print("Searching for {}".format(pattern))
    try:
        while True:
            token = (yield)
            if pattern in token:
                next_coroutine.send(token)
    except GeneratorExit:
        print("Done with filtering!!")


def print_token():
    print("I'm sink, i'll print tokens")
    try:
        while True:
            token = (yield)
            print(token)
    except GeneratorExit:
        print("Done with printing!")


pt = print_token()
next(pt)
pf = pattern_filter(next_coroutine=pt)
next(pf)

sentence = "Bob is running behind a fast moving car"
producer(sentence, pf)
```

运行结果：

```python
I'm sink, i'll print tokens
Searching for ing
running
moving
Done with filtering!!
Done with printing!
```

[^first]: [廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/897692888725344/923057403198272#0)
    
[^second]: [GeeksForGeeks](https://www.geeksforgeeks.org/coroutine-in-python/)
