---
title: "Python包的__path__变量"
date: 2019-07-05
draft: false
---
在包的`__init__.py`中，`__path__`变量指定包的搜索路径。`__path__[0]`默认为空，Pycharm中会将`__path__[0]`改为项目的根目录，以便我们可以用绝对路径的方式导入模块。

当需要在运行时确定使用哪一套配置时，`__path__`可以派上用场。如：

```python
env = os.environ.get('ENV','dev')
__path__ = os.path.abspath(os.path.join(__path__[0], env))
```
