---
title: "ARTS打卡 - 第一周"
date: 2023-08-21T22:27:21+08:00
draft: false
---

# 前言
我不是一个喜欢凑热闹的人, 越是热闹的东西，我越是不碰，似乎要与全世界作对，我不玩抖音、不玩微博等乱七八糟的东西，那铺面而来的信息令我惶恐不安。

但是这次不一样，陈皓老师在我前进的道路上给了我精神上很大的鼓舞，我还记得大约三年前，刚转正正式参加工作的时候，看着陈皓博客的文章，让我热血沸腾的感觉，心想有一天我也要成为这么牛逼的程序员。虽然我现在还远远没有达到，但是这种信念似乎还一直在我心中，这个更加重要。

多说几句吧

刚开始工作第一年，业余时间基本都在按照陈皓老师的专栏中介绍的路径在补充学习，汇编，unp，apue等，虽然现在几乎全忘光了，但是我怀念那种感觉。

陈皓老师去世了，世事无常阿卧槽！


# 1、Algorithm
## 颜色分类(荷兰国旗问题)
给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

注意:
不能使用代码库中的排序函数来解决这道题。

进阶：
一个直观的解决方案是使用计数排序的两趟扫描算法。
首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
你能想出一个仅使用常数空间的一趟扫描算法吗？

### 示例
``` 
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```

### 思路
双指针, 开始位置分别位于数组两端, 分别分割0和2的区域, 从左向右遍历，将0交换到左边, 将2交换到右边，然后处理一下终止和边界情况。

### 代码
``` python3
class Solution:
    def sortcolors(self, nums: List[int]) -> None:
        n = len(nums)
        p0, p2 = 0, n - 1
        i = 0
        while i <= p2:
            while i <= p2 and nums[i] == 2:
                nums[i], nums[p2] = nums[p2], nums[i]
                p2 -= 1
            if nums[i] == 0:
                nums[i], nums[p0] = nums[p0], nums[i]
                p0 += 1
            i += 1
```


# 2、Review
分享一下medium中一篇关于[规则引擎介绍的文章](https://medium.com/@er.rameshkatiyar/what-is-rule-engine-86ea759ad97d)
> Logic:
A person is eligible for car loan if, he has monthly salary more than 70K and his credit score is more than 900 then, approve the car loan and sanction the 60% of requested amount.
You can easily implement these types of rules or logic in your application. But If you will get some additional requirements like:
* If there are a large number of logics then, how you will search and apply them efficiently? (Good performance.)
* If logics are frequently changing and you generally code your logic in the application, then how you will manage or change the code that frequently? (Avoid frequent deployment.)
* Design the application such that, it can be easily maintained and understood by business people. (Use by non-technical members)
* If you have to keep your all business logic at a centralized place and separate from all the applications then, where you will keep it?

To achieve all these requirements in our application, we can use the rule-engine. 

为了在我们的应用程序中实现所有这些要求，我们可以使用规则引擎。

Terminologies:
> Rule: It is a set of the condition followed by the set of actions. It represents the logic of the system. The rules are mainly represented in the if-then form. It contains mainly two parts, condition, and action. The rule is also known as production.
> Rule = Condition + Action

规则 = 条件 + 动作

# 3、Technique/Tips
最近在验证chatgpt的落地场景，有一些调prompt的实际经验，简单分享几点:

1、对问题的描述要清晰、具体，这是最重要的

2、当你试图详细解释/举例一个你自己都无法想明白的概念/场景时, 请不要解释/举例，使用Zero Shot COT

3、当你把一个prompt越调越复杂时, 请冷静下来仔细分析, 有必要这么复杂吗

# 4、Share
沟通很重要, 程序员在初期一般不会认识到这一点, 遇到需要沟通的场景时总是想办法回避之，想着俺要一心修炼技术，并在心里默念`Talk is cheap, show me the code`， 然后沾沾自喜，觉得自己十分之聪明。

俺之前就是如此，现在正在慢慢纠正。
