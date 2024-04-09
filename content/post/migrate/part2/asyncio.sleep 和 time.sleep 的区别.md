---
title: "asyncio.sleep 和 time.sleep 的区别"
date: 2020-05-10
draft: false
tags:
  - Python
  - asyncio
---
`time.sleep`是针对整个线程，整个线程会挂起，不再执行任何操作。

`asyncio.sleep`是针对当前协程而言，告诉事件循环：请去执行别的操作，相当于模拟了一次网络IO，不会阻塞其他协程的执行。

```python3
import time
import asyncio

async def hello():
    print('Hello ...')
    await asyncio.sleep(5)
    # time.sleep(5)
    print('... World!')

async def main():
    await asyncio.gather(hello(), hello())

loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```

运行结果：

```python3
Hello ...
Hello ...
... World!
... World!
```

```python3
import time
import asyncio

async def hello():
    print('Hello ...')
    # await asyncio.sleep(5)
    time.sleep(5)
    print('... World!')

async def main():
    await asyncio.gather(hello(), hello())

loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```

运行结果：

```python3
Hello ...
... World!
Hello ...
... World!
```
