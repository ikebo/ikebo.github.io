---
title: "v8与webkit的关系"
date: 2021-09-08
draft: false
tags:
  - v8引擎
  - webkit
---
# Part1[^first]

Chrome页面的绘制（绘制，就是把一个HTML文件变成一个活灵活现的页面展示的过程...），只有一半轮子是Chrome自己做的，还有一部分来自于WebKit，这个Apple打造的Web渲染器。。。

之所以说是一半轮子来源于WebKit，是因为WebKit本身包含两部分主要内容，一部分是做Html渲染的，另一部分是做JavaScript解析的。在Chrome中，只有Html的渲染采用了WebKit的代码，而在JavaScript上，重新搭建了一个NB哄哄的V8引擎。目标是，用WebKit + V8的强强联手，打造一款上网冲浪的法拉利，从效果来看，还着实做的不错。。。

不过，虽说Chrome和WebKit都是开源的，并联手工作。但是，Chrome还是刻意的和WebKit保持了距离，为其始乱终弃埋下了伏笔。Chrome在WebKit上封装了一层，称为WebKit Glue。Glue层中，大部分类型的结构和接口都和WebKit类似，Chrome中依托WebKit的组件，都只是调用WebKit Glue层的接口，而不是直接调用WebKit中的类型。按照Chrome自己文档中的话来说，就是，虽然我们再用WebKit实现页面的渲染，但通过这个设计（加一个间接层...）已经从某种程度大大降低了与WebKit的耦合，使得可以很容易将WebKit换成某个未来可能出现的更好的渲染引擎。。。

# Part2[^second]

我们知道不同浏览器用的不同的渲染引擎：

Tridend(IE)、Gecko(FF)、WebKit(Safari,Chrome,Andriod浏览器)

当然 Chrome 重构了一下 WebKit 然后管它叫 Blink。但是大体架构还是和 WebKit 一致的。

我们看看我们常说的 V8 和 WebKit 有什么关系吧。

下面是 WebKit 的大致结构：

![](/uploads/2021/12/1012942913.jpg)

实线框内模块是所有移植的共有部分，虚线框内不同的厂商可以自己实现。

就是说 JS 引擎(JS 虚拟机)，WebKit 是默认的是 JSCore，而 Google 则自己实现了一版吊炸天的 V8。

因此虽然同样是WebKit，Safari 用的是 JSCore, Chrome 用的是 V8。

[^first]: [v8与webkit的关系](https://www.cnblogs.com/liyanfeng/articles/8036970.html)
    
[^second]: [webkit vs v8](https://www.cnblogs.com/amiezhang/p/11443867.html)
