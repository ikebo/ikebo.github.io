---
title: "Arts 2"
date: 2023-08-28T23:58:50+08:00
draft: false
---


# 1、Algorithm
## 换水问题
超市正在促销，你可以用 numExchange 个空水瓶从超市兑换一瓶水。最开始，你一共购入了 numBottles 瓶水。

如果喝掉了水瓶中的水，那么水瓶就会变成空的。

给你两个整数 numBottles 和 numExchange ，返回你 最多 可以喝到多少瓶水。


### 示例
示例 1：
![eg1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/sample_1_1875.png)

>
输入：numBottles = 9, numExchange = 3
输出：13
解释：你可以用 3 个空瓶兑换 1 瓶水。
所以最多能喝到 9 + 3 + 1 = 13 瓶水。


示例2
![eg2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/sample_2_1875.png)
>输入：numBottles = 15, numExchange = 4
输出：19
解释：你可以用 4 个空瓶兑换 1 瓶水。
所以最多能喝到 15 + 3 + 1 = 19 瓶水。


提示：
* 1 <= numBottles <= 100
* 2 <= numExchange <= 100


### 思路
首先我们一定可以喝到 bbb 瓶酒，剩下 bbb 个空瓶。接下来我们可以拿瓶子换酒，每次拿出 eee 个瓶子换一瓶酒，然后再喝完这瓶酒，得到一个空瓶。以此类推，我们可以统计得到答案。


### 代码
``` python3
class Solution:
    def numWaterBottles(self, numBottles: int, numExchange: int) -> int:
        bottle, ans = numBottles, numBottles
        while bottle >= numExchange:
            bottle -= numExchange
            ans += 1
            bottle += 1
        return ans
```


# 2、Review
分享一篇medium中关于架构模式的文章，[10种软件架构的常见模式](https://towardsdatascience.com/10-common-software-architectural-patterns-in-a-nutshell-a0b47a1e9013)
> An architectural pattern is a general, reusable solution to a commonly occurring problem in software architecture within a given context. Architectural patterns are similar to software design pattern but have a broader scope.

In this article, I will be briefly explaining the following 10 common architectural patterns with their usage, pros and cons.

* Layered pattern
* Client-server pattern
* Master-slave pattern
* Pipe-filter pattern
* Broker pattern
* Peer-to-peer pattern
* Event-bus pattern
* Model-view-controller pattern
* Blackboard pattern
* Interpreter pattern


# 3、Technique/Tips
cursor + github copilot 结合使用，日常编码效率大大提高，一些与业务相关性不大的逻辑更明显，推荐使用！


# 4、Share
1、AIGC的相关技术正在带来一场巨大变革，不要抗拒，不要固步自封，拥抱新技术吧

2、早点睡觉，好好活着是最重要的。
