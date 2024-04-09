---
title: "Python 在CSV文件中写入中文字符"
date: 2019-09-25
draft: false
tags:
  - Python
---
对于UTF-8编码，Excel要求BOM(字节顺序标记)写在文件的开始，否则它会假设这是ANSI编码，这个就是与locale有依赖性了。

Python2

```python
#!python2
#coding:utf8
import csv

data = [[u'American',u'美国人'],
        [u'Chinese',u'中国人']]

with open('results.csv','wb') as f:
    f.write(u'\ufeff'.encode('utf8'))
    w = csv.writer(f)
    for row in data:
        w.writerow([item.encode('utf8') for item in row])
```

Python3

```python
#!python3
#coding:utf8
import csv

data = [[u'American',u'美国人'],
        [u'Chinese',u'中国人']]

with open('results.csv','w',newline='',encoding='utf-8-sig') as f:
    w = csv.writer(f)
    w.writerows(data)
```

[unicodecsv](https://link.jianshu.com/?t=https://github.com/jdunck/python-unicodecsv)

```python
#!python2
#coding:utf8
import unicodecsv

data = [[u'American',u'美国人'],
        [u'Chinese',u'中国人']]

with open('results.csv','wb') as f:
    w = unicodecsv.writer(f,encoding='utf-8-sig')
    w.writerows(data)
```

转载自[简书](https://www.jianshu.com/p/87b60b696780)
