---
title: "使用Privoxy转发http到socks"
date: 2021-04-29
draft: false
tags:
  - privoxy
  - socks
  - http
---
国内服务器ping不通github，正好有一台香港的socks server，如果可以将http转发到这台server，问题就解决了。

privoxy是一款不进行网页缓存且自带过滤功能的代理服务器，针对http、https协议。通过其过滤功能，用户可以保护隐私、对网页内容进行过滤、管理Cookie，以及拦阻各种广告等。[^first]

[privoxy官网](https://www.privoxy.org)

大概意思是从http协议到其他协议的转换，另外可以做一些http内容的过滤、修改等。

安装privoxy（ubuntu）

`apt-get install privoxy`

默认privoxy服务已经通过systemctl管理，`systemctl status/stop/start privoxy`查看/停止/启动privoxy

配置文件在`/etc/privoxy/config`

在文件末尾加上`forward-socks5 / host:port .`

将host:port替换成你的`socks local` server  最后按个.表示转发到socks server之后不用再转发到某个http server了

如果想这个privoxy服务可以被外部访问的话，比如本机通过这个privoxy进行科学上网，可以将配置文件中`listen-address` 的`localhost`改成`0.0.0.0`

最后，重启服务: `systemctl restart privoxy`

[^first]: https://zh.wikipedia.org/wiki/Privoxy
