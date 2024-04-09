---
title: "一键搭建ssr服务器并开启bbr加速"
date: 2019-08-13
draft: false
tags:
  - ssr
---
一键搭建shadowsocksR

1 下载ssr搭建脚本

```shell
git clone -b master https://github.com/flyzy2005/ss-fly
```

2 运行ssr搭建脚本

```shell
ss-fly/ss-fly.sh -ssr
```

3 输入对应参数

4 相关命令

```shell
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status
配置文件路径：/etc/shadowsocks.json
日志文件路径：/var/log/shadowsocks.log
代码安装目录：/local/shadowsocks
```

5 卸载ssr服务

```shell
./shadowsocksR.sh uninstall
```

## 一键开启bbr加速

```shell
ss-fly/ss-fly.sh -bbr
```

重启，输入以下命令查看是否成功开启bbr

```shell
sysctl net.ipv4.tcp_available_congestion_control
```

如果返回值为：

```shell
net.ipv4.tcp_available_congestion_control = bbr cubic reno
```

则说明开启成功

---

转载自[这里](https://github.com/flyzy2005/ss-fly/wiki/%E4%B8%80%E9%94%AE%E8%84%9A%E6%9C%AC%E6%90%AD%E5%BB%BASS-%E6%90%AD%E5%BB%BASSR%E6%9C%8D%E5%8A%A1%E5%B9%B6%E5%BC%80%E5%90%AFBBR%E5%8A%A0%E9%80%9F)
