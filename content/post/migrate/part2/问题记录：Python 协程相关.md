---
title: "问题记录：Python 协程相关"
date: 2020-05-14
draft: false
tags:
  - Python
  - coroutine
---
```python3
import asyncio
import multiprocessing

q = multiprocessing.Queue(10000)

for i in range(100):
    q.put(i)

async def coro(i):
    print('coro... {}'.format(i))

async def device_video_main(j):
    loop = asyncio.get_event_loop()
    for i in range(5):
        asyncio.ensure_future(coro(j), loop=loop)
        # await asyncio.sleep(1)

async def run_integrate():
    while True:
        j = q.get()
        print('j: ', j)
        if True:
            loop = asyncio.get_event_loop()
            coro = device_video_main(j)
            loop.create_task(coro)
            # asyncio.ensure_future(coro, loop=loop)
            # await asyncio.sleep(1)
        # print(loop.is_running())

# async def main():
#     loop = asyncio.get_event_loop()
#     for i in range(3):
#         asyncio.ensure_future(device_video_main(i), loop=loop)
#         # await asyncio.sleep(2)
#         # loop.run_forever()
#     await asyncio.sleep(10)

async def test():
    await run_integrate()

def async_main():
    loop = asyncio.get_event_loop()
    # future = loop.create_future()
    task = loop.create_task(run_integrate())
    loop.run_forever()
    # print(task)
    # future = asyncio.Future()
    # future.
    # loop.run_until_complete(test())


if __name__ == "__main__":
    process = multiprocessing.Process(target=async_main)
    process.start()
```
