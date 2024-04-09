---
title: "Python中的async/await"
date: 2019-07-09
draft: false
tags:
  - Python
  - async
  - await
---
`async/await`是实现异步IO的语法糖，是`Python3.7`新出的关键字。`async def`可创建协程，而`await`可用来等待一个可等待对象的执行完成。这大大简化了协程的创建(在Python2中创建协程需要`yield`和`send`协同操作)

下面这个例子很简洁的说明了什么是异步IO[^first]：

```python
import asyncio

async def count():
    print("One")
    await asyncio.sleep(1)
    print("Two")

async def main():
    await asyncio.gather(count(), count(), count())

if __name__ == "__main__":
    import time
    s = time.perf_counter()
    asyncio.run(main())
    elapsed = time.perf_counter() - s
    print(f"{__file__} executed in {elapsed:0.2f} seconds.")
```

运行结果：

```shell
$ python3 countasync.py
One
One
One
Two
Two
Two
countasync.py executed in 1.01 seconds.
```

`gather`会等待所有协程都返回后再返回一个结果列表，`as_completed`会当协程返回后立即返回：

```python
>>> import asyncio

>>> async def coro(seq) -> list:
...     """'IO' wait time is proportional to the max element."""
...     await asyncio.sleep(max(seq))
...     return list(reversed(seq))

>>> async def main():
...     t = asyncio.create_task(coro([3, 2, 1]))
...     t2 = asyncio.create_task(coro([10, 5, 0]))
...     print('Start:', time.strftime('%X'))
...     for res in asyncio.as_completed((t, t2)):
...         compl = await res
...         print(f'res: {compl} completed at {time.strftime("%X")}')
...     print('End:', time.strftime('%X'))
...     print(f'Both tasks done: {all((t.done(), t2.done()))}')
...
>>> a = asyncio.run(main())
Start: 09:49:07
res: [1, 2, 3] completed at 09:49:10
res: [0, 5, 10] completed at 09:49:17
End: 09:49:17
Both tasks done: True 
```


[^first]: [RealPython](https://realpython.com/async-io-python/)