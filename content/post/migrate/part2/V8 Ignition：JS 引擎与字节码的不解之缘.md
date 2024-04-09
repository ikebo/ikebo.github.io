---
title: "V8 Ignition：JS 引擎与字节码的不解之缘"
date: 2021-09-08
draft: false
tags:
  - v8引擎
  - Ignition
---
[http://arewefastyet.com](http://arewefastyet.com/) 网站测试并展示了数个 JavaScript 引擎的性能数据，是各家 JS 引擎性能的比武场：

![1.png](https://static.cnodejs.org/Fvi10Dp8Gq_hwN3z4UKVElf9YvAM)

我们看到在这个比武场上，最近 Chrome 出现了多个新条目，其中很多条目都是关于 v8 的 Ignition 新架构的组合，他们是 v8 引擎最近推出的 JS 字节码解释器。

纵览各个 JS 引擎的实现，我们发现基于字节码的实现是主流。例如苹果公司的 JavaScriptCore （JSC） 引擎，2008 年时他们引入了 SquirrelFish（市场名 Nitro），实现了一个字节码寄存器机（Register Machine）。再如 Mozilla 公司的 SpiderMonkey，他们使用字节码的历史更久，可以追溯到 1998 年的 Netscape 4（见 [https://dxr.mozilla.org/classic/source/js/src/jsemit.c](https://dxr.mozilla.org/classic/source/js/src/jsemit.c) ），SpiderMonkey 实现的是堆栈机（Stack Machine）。微软的 Chakra 也使用了字节码，他们实现的是寄存器机（Register Machine）。而 v8 之前的做法是比较“脱俗”的，他们跳过了字节码这一层，直接把 JS 编译成机器码。而在刚刚过去的五一假日前夕，v8 5.9 发布了，其中的 Ignition 字节码解释器将默认启动 ：[https://v8project.blogspot.co.id/2017/04/v8-release-59.html](https://v8project.blogspot.co.id/2017/04/v8-release-59.html)  。v8 自此回到了字节码的怀抱。

这让笔者不禁怀念起 2007 年 Ruby 1.9 的发布。当时 Ruby 1.9 也是第一次引入了字节码，名为 YARV，由笹田耕一领导主导开发完成。当时，Ruby 还在使用松本行弘的初级的解释器实现，亦即，解释器每次遍历代码的抽象语法树（AST）来进行 Ruby 代码的解释执行。而 YARV 则把抽象语法树（AST）先编译成字节码，然后再运行。引入字节码之后，Ruby 的性能得到了显著的提升。

而这次 V8 引入字节码却是向着相反的方向后退。因为之前 v8 选择了直接将 JS 代码编译到机器代码执行，机器码的执行性能已经非常之高，而这次引入字节码则是选择编译 JS 代码到一个中间态的字节码，执行时是解释执行，性能是低于机器代码的。最终的性能测试势必会降低，而不是提高。那么 V8 为什么要做这样一个退步的选择呢？为 V8 引入字节码的动机又是什么呢？笔者总结下来有三条：

* （主要动机）减轻机器码占用的内存空间，即牺牲时间换空间
* 提高代码的启动速度
* 对 v8 的代码进行重构，降低 v8 的代码复杂度

故事得从 Chrome 的一个 bug 说起： [http://crbug.com/593477](http://crbug.com/593477) 。Bug 的报告人发现，当在 Chrome 51 (canary) 浏览器下加载、退出、重新加载 facebook 多次，并打开 about:tracing 里的各项监控开关，可以发现第一次加载时 v8.CompileScript 花费了 165 ms，再次加载加入 V8.ParseLazy 居然依然花费了 376 ms。按说如果 Facebook 网站的 js 脚本没有变，Chrome 的缓存功能应该缓存了对 js 脚本的解析结果，不该花费这么久。这是为什么呢？

这就是之前 v8 将 JS 代码编译成机器码所带来的问题。因为机器码占空间很大，v8 没有办法把 Facebook 的所有 js 代码编译成机器码缓存下来，因为这样不仅缓存占用的内存、磁盘空间很大，而且退出 Chrome 再打开时序列化、反序列化缓存所花费的时间也很长，时间、空间成本都接受不了。

所以 v8 退而求其次，只编译最外层的 js 代码，也就是下图这个例子里面绿色的部分。那么内部的代码（如下图中的黄色、红色的部分）是什么时候编译的呢？v8 推迟到第一次被调用的时候再编译。这时间上的推移还导致另外一个短板，就是代码必须被解析多次——绿色的代码一次、黄色的代码再解析一次（当 new Person 被调用）、红色的代码再解析一次（当 doWork() 被调用）。因此，如果你的 js 代码的闭包套了 n 层，那么最终他们至少会被 v8 解析 n 次。

![2.png](https://static.cnodejs.org/FhKw46OcBhzAeyUk2mkf0hj8GsKK)

Facebook 的网站之所以收到这个设计带来的负面的性能影响，就是因为他们的前段工程流程中最后把各个独立的 module 编译成了一个单独的文件，其中用到了很多闭包，如：

![3.png](https://static.cnodejs.org/FhfnB40Ll8-Y_ItlaPMgELrwgemE)

如此一来 Chrome 的缓存作用就只能作用在最外层的 `__d()` 代码上，而内部的真正的逻辑根本没有被缓存。

刚才提到了机器码占空间大的一个坏处，就是不能一次性编译全部的代码。机器码占空间大还有另外一个坏处，就是一些只运行一次的代码浪费了宝贵的内存资源。正如上面 Facebook 中的 `__d()` 系列函数，他们的作用可能只是注册、初始化各个模块组件，而一旦初始化完成便不会再执行。但由于机器码占空间大，这些只执行一次的代码也会在内存中长期存在、长期占用空间。正如下图所示，一般情况下大约 30% 的 V8 堆空间都用来存储未优化的机器码。

![4.png](https://static.cnodejs.org/FslD2wo_cAHHCgMtZgH804t8VkED)

而引入字节码之后，占空间的问题就可以得到缓解。通过恰当地设计字节码的编码方式，字节码可以做到比机器码紧凑很多。V8 引入 Ignition 字节码后，代码的内存占用确实降低了，如下图所示。

![5.png](https://static.cnodejs.org/FjCXv1xd4uqHfsyDXtWCHhpYZrQy)

通过对十大流行手机端网站的测试，可以发现他们的内存占用显著下降。

![6.png](https://static.cnodejs.org/FtK_cj15jt5L6nKUlT8bA1hvUhO4)

这便是 v8 引入字节码的主要动机。而这样实现之后其实顺便又带来了两个好处，笔者认为可以视作 v8 引入字节码的次要动机，亦即：更快的启动速度和更好的 v8 代码重构。

在启动速度方面，如今内存占用过大的问题消除了，就可以提前编译所有代码了。因为前端工程为了节省网络流量，其最终 JS 产品往往不会分发无用的代码，所以可以期望全部提前编译 JS 代码不会因为编译了过多代码而浪费资源。v8 对于 Facebook 这样的网站就可以选择全部提前编译 JS 代码到字节码，并把字节码缓存下来，如此 Facebook 第二次打开的时候启动速度就变快了。下图是旧的 v8 的执行时间的统计数据，其中 33% 的解析、编译 JS 脚本的时间在新架构中就可以被缩短。

![7.png](https://static.cnodejs.org/FoRWjvkPVtzUP4_FaP51Qss0eHEA)

v8 自身的重构方面，有了字节码，v8 可以朝着简化的架构方向发展，消除 Cranshaft 这个旧的编译器，并让新的 Turbofan 直接从字节码来优化代码，并当需要进行反优化的时候直接反优化到字节码，而不需要再考虑 JS 源代码。最终达到如下图所示的架构。

![8.png](https://static.cnodejs.org/FqkEysEN0yOK7OpWwqGNKeAhqa9h)

其实，Ignition + TurboFan 的组合，就是字节码解释器 + JIT 编译器的黄金组合。这一黄金组合在很多 JS 引擎中都有所使用，例如微软的 Chakra，它首先解释执行字节码，然后观察执行情况，如果发现热点代码，那么后台的 JIT 就把字节码编译成高效代码，之后便只执行高效代码而不再解释执行字节码。苹果公司的 SquirrelFish Extreme 也引入了 JIT。SpiderMonkey 更是如此，所有 JS 代码最初都是被解释器解释执行的，解释器同时收集执行信息，当它发现代码变热了之后，JaegerMonkey、IonMonkey 等 JIT 便登场，来编译生成高效的机器码。

回顾历史，很多 JS 引擎都是采用了字节码这一脚本语言实现技术的，而 v8 一枝独秀，走“纯机器码”路线，其实过于激进了：虽然执行性能上可以登峰造极，但却带来了内存占用过大的问题。这次引入字节码实则是做了工程上的恰当取舍，将损失掉的内存找回来，更加符合如今移动和嵌入式设备为主的应用场景；以时间换空间，让 v8 能更好的服务于低内存的设备。如今 V8 也回到了字节码的怀抱，不禁令人感叹 JS 引擎与字节码真是有着不解之缘！

转载自[cnnodejs](https://cnodejs.org/topic/59084a9cbbaf2f3f569be482)
