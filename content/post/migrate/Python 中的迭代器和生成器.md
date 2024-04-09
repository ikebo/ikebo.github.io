---
title: "Python 中的迭代器和生成器"
date: 2019-07-04
draft: false
tags:
  - Python
  - 迭代器
  - 生成器
---
Python的迭代器和生成器属于比较难理解的内容，不常使用的话又容易忘记。最近在看一些代码的时候经常看到`__iter__`和`yield`，发现如果想深入理解的话并不简单。

## 迭代器

Python中，能够通过for...in循环遍历的对象都是可迭代对象`iterable objects`, 如`list`, `dict`等。迭代器是指实现了迭代协议的对象, 可通过`next()`函数调用并返回下一个值，直到最后抛出`StopIteration`错误。可迭代对象不一定是迭代器，可通过`iter()`函数将可迭代对象变成迭代器。

如：

```python
class yrange:
    def __init__(self, n):
        self.i = 0
        self.n = n

    def __iter__(self):
        return self

    def next(self):
        if self.i < self.n:
            i = self.i
            self.i += 1
            return i
        else:
            raise StopIteration()
```

运行结果：

```python
>>> y = yrange(3)
>>> y.next()
0
>>> y.next()
1
>>> y.next()
2
>>> y.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 14, in next
StopIteration
```

## 生成器

生成器简化了迭代器的创建。如：

```python
def yrange(n):
    i = 0
    while i < n:
        yield i
        i += 1
```

运行结果：

```python
>>> y
<generator object yrange at 0x401f30>
>>> y.next()
0
>>> y.next()
1
>>> y.next()
2
>>> y.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

## 生成器使用示例

类似`cat`:

```python
def cat(filenames):
    for f in filenames:
        for line in open(f):
            print line,
```

类似`grep`

```python
def grep(pattern, filenames):
    for f in filenames:
        for line in open(f):
            if pattern in line:
                print line,
```

这两个函数都有很多相似代码，但是又很难将相似部分作为函数提取出来。这时生成器就能很好地派上用场了：

```python
def readfiles(filenames):
    for f in filenames:
        for line in open(f):
            yield line

def grep(pattern, lines):
    return (line for line in lines if pattern in line)

def printlines(lines):
    for line in lines:
        print line,

def main(pattern, filenames):
    lines = readfiles(filenames)
    lines = grep(pattern, lines)
    printlines(lines)
```

参考[Python practice book](https://anandology.com/python-practice-book/iterators.html)