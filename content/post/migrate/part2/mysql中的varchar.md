---
title: "mysql中的varchar"
date: 2019-07-05
draft: false
tags:
  - mysql
---
长度范围是0到65535

## varchar(255) 和 varchar(256)的区别

长度超过255时，用2个字节存储**列的实际长度**，未超过时用一个字段
