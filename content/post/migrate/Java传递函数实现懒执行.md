---
title: "Java传递函数实现懒执行"
date: 2021-03-25
draft: false
tags:
  - java
  - lambda
  - lazy
---
看下面这个方法:

```java
public static JSONObject call(byte[] img, String algName, String algUrl) {
    long startTime = System.currentTimeMillis();
    try {
        HttpResponse resp = OkHttpUtil.post(algUrl, MediaType.parse("application/octet-stream"), img, connectTimout, readAndWriteTimeout);
        // 省略代码块...
    } catch (Exception e) {
        // 省略代码块...
    }
}
```

方法的入参是图片二进制流，模型名称和模型地址，方法的操作是用图片调用对应的模型，然后做一些日志记录、监控指标上报、错误处理等操作，最终返回模型的json格式结果。

可以看到其中模型的调用方式是确定的，直接将图片放在`http body`中，`content-type`为`application/octet-stream`

现在有一个模型，调用方式不一样，需要以`form-data`的格式把数据传过去，图片的名字必须为`image`, 像这样:

```java
OkHttpUtil.post(url, null, "image", img, null, 10000L, 10000L)
```

两者唯一的区别只是调用方式不一样，如果能把调用方式抽象出来单独传递的话，那是极好的。像Python这类的动态语言是很容易实现的，直接将函数作为参数传递，之后用`()`调用运算符调用即可：

```python
def func():
    return "world"

def outter(fun):
    print("hello, {}".format(fun()))

outter(func)  # "hello, world"
```

联想到Java中是否也可以这样呢，Java也是可以支持函数式编程的，自然也是可以的。实现方式如下

定义一个函数式接口:

```java
@FunctionalInterface
public interface Lazy<T> {
    T value();
}
```

增加一个重载`call`方法，需要额外传入`Lazy`类型的参数`httpHolder`

重载call方法如下：

```java
public static JSONObject call(byte[] img, String algName, String algUrl, Lazy<HttpResponse> httpHolder) {
    long startTime = System.currentTimeMillis();
    try {
        HttpResponse resp = httpHolder.value();
        // 省略代码块...
    } catch (Exception e) {
        // 省略代码块...
    }
}
```
