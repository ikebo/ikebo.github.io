---
title: "aiohttp ClientSession 用法踩坑"
date: 2020-09-01
draft: false
tags:
  - Python
  - aiohttp
---
一般是这么用的：

```python
async with aiohttp.ClientSession() as ses:
	res = await ses.post(xxx)
	text = await res.text()
	xxx
```

没有问题

但是为了减少缩进，可能想这样封装一下：

```python
class HTTP:
	@staticmethod
	async def post(*args, **kwargs):
		async with aiohttp.ClientSession() as ses:
			return await ses.post(*args, **kwargs)
```

这是有问题的，with 上下文之后会关闭session的连接和资源，如果payload比较大，在连接关闭之后还没读完的话，可能会卡在await ses.text()那里，导致超时

所以需要在上下文关闭之前就把内容读取完毕并返回。

可以这样：

```python
class HTTP:
	@staticmethod
	async def post(*args, **kwargs):
		async with aiohttp.ClientSession() as ses:
			async with await ses.post(*args, **kwargs) as res:
				return res, await res.text()
```

或者这样：

```python
class HTTP:
	@classmethod
    async def get(cls, *args, **kargs):
        await cls.init()
        return await asyncio.ensure_future(cls.client.get(*args, **kargs))
```
