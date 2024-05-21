---
title: "Nginx Proxy Manager 单机多Docker Compose 反向代理配置"
date: 2024-05-21T19:17:55+08:00
draft: false
tags:
 - docker
 - docker compose
---

# 问题描述
单机下部署多个docker-compose, 并用nginx proxy manager 反代其他docker compose的流量，出现连不上其他docker compose 下的container的问题


# 尝试过的解决方式

1、地址用docker0的地址，端口用其他container暴露出来的端口，不行

2、换机器，不行

3、换docker、docker-compose 版本，不行

# 最终的解决方式
手动将多个docker compose加入到同一个docker network.

具体参考:</br>
1、https://nginxproxymanager.com/advanced-config</br>

2、https://stackoverflow.com/questions/38088279/communication-between-multiple-docker-compose-projects</br>

3、https://github.com/NginxProxyManager/nginx-proxy-manager/issues/555


## 05.20 更新
最终还是要解决容器内无法访问宿主机IP的问题，非常诡异：</br>
1、容器内可以ping通其他容器的hostname, 端口也是通的</br>
2、容器内可以ping通宿主机ip (eth0)</br>
3、但是容器无法连上宿主机的端口</br>
4、公网可以正常访问宿主机的服务(端口是通的)</br>

排查一圈发现是防火墙的原因，Ubuntu 22.04 TLS,  记得要把防火墙关了！(ufw disable)


艹！

参考过的几个有用文章：</br>
1、https://juejin.cn/post/7180619106407120933</br>
2、https://gist.github.com/bruno-brant/e119da3713a657036ff7e3446d98176a

一般软件打的镜像都比较轻量，用的alpine版的linux比较多，进容器后用apk update && apk add... 装一些常用软件，方便排查问题。
