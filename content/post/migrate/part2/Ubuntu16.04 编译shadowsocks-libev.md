---
title: "Ubuntu16.04 编译shadowsocks-libev"
date: 2021-08-05
draft: false
tags:
  - shadowsocks
---
记录一下

1. 安装依赖

```shell
apt install -y gettext build-essential autoconf pkg-config libtool libpcre3-dev asciidoc xmlto libev-dev libc-ares-dev automake libmbedtls-dev libsodium-dev
```

2. 下载代码

```shell
git clone https://github.com/shadowsocks/shadowsocks-libev.git
cd shadowsocks-libev
git submodule update --init --recursive //下载子模块
```

3. 编译

```shell
./autogen.sh
./configure
make && make install
```
