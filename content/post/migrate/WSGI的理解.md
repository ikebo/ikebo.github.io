---
title: "WSGI的理解"
date: 2019-07-04
draft: false
tags:
  - wsgi
  - uwsgi
---
WSGI(Web Server Gateway Interface)是描述Python应用和服务器之间标准接口的协议。若应用开发者和服务器开发者都实现这个协议，则双方都只需要专注自己所需要开发的功能，而不用考虑应用／服务器兼容的问题。

目前WSGI协议已经得到广泛实现, WSGI应用/框架有flask, django等，WSGI服务器有uWSGI(其实现的uwsgi协议是传输协议，主要用于与反向代理的通信), gunicorn等。

WSGI在[PEP333](https://www.python.org/dev/peps/pep-0333/)中发布，主要内容为[^first]：

> * WSGI application are callable python objects (functions or classes with a __call__
>   method that are passed two arguments: a WSGI environment as first argument and a
>   function that starts the response.
> * the application has to start a response using the function provided and return an iterable > where each yielded item means writing and flushing.
> * The WSGI environment is like a CGI environment just with some additional keys that are either provided by the server or a middleware.
> * you can add middlewares to your application by wrapping it.

## 路由转发示例

```python
# path dispatching

import re
from cgi import escape


def index(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return [''' Hello World Application
                This is the Hello World application
    ''']


def hello(environ, start_response):
    args = environ['myapp.url_args']
    if args:
        subject = escape(args[0])
    else:
        subject = 'World'
    start_response('200 OK', [('Content-Type', 'text/html')])
    return ['''Hello %(subject)s
    ''' % {'subject': subject}]


def not_found(environ, start_response):
    start_response('404 NOT FOUND', [('Content-Type', 'text/plain')])
    return ['Not Found']


urls = [
    (r'^$', index),
    (r'hello/?$', hello),
    (r'hello/(.+)$', hello)
]


def application(environ, start_response):
    path = environ.get('PATH_INFO', '').lstrip('/')
    for regex, callback in urls:
        match = re.search(regex, path)
        if match is not None:
            environ['myapp.url_args'] = match.groups()
            return callback(environ, start_response)
    return not_found(environ, start_response)

if __name__ == '__main__':
    from wsgiref.simple_server import make_server
    server = make_server(
        'localhost',
        5000,
        application
    )
    server.serve_forever()
```

## Middleware示例

```python
class ExceptionMiddleware(object):
    def __init__(self, app):
        self.app = app

    def __call__(self, environ, start_response):
        appiter = None
        try:
            appiter = self.app(environ, start_response)
            for item in appiter:
                yield item
        except:
            e_type, e_value, tb = exc_info()
            traceback = ['Traceback (most recent call last):']
            traceback += format_tb(tb)
            traceback.append('%s: %s' % (e_type.__name__, e_value))

            try:
                start_response('500 INTERVAL SERVER ERROR', [
                    ('Content-Type', 'text/plain')])
            except:
                pass
            yield '\n'.join(traceback)

        if hasattr(appiter, 'close'):
            appiter.close()

# 使用方式：ExceptionMiddleware(application)

```

[^first]: [pocoo](http://lucumr.pocoo.org/2007/5/21/getting-started-with-wsgi/)
