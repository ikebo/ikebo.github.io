---
title: "ARTS打卡-第三周"
date: 2023-09-04T20:47:59+08:00
draft: false
---


# 1、Algorithm
## 和为 K 的子数组
给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的连续子数组的个数 。

 

### 示例 1：

输入：nums = [1,1,1], k = 2
输出：2

### 示例 2：

输入：nums = [1,2,3], k = 3
输出：2
 

### 提示：

* 1 <= nums.length <= 2 * 104
* -1000 <= nums[i] <= 1000
* -107 <= k <= 107


### 思路
我们可以基于方法一利用数据结构进行进一步的优化，我们知道方法一的瓶颈在于对每个 iii，我们需要枚举所有的 jjj 来判断是否符合条件，这一步是否可以优化呢？答案是可以的。

我们定义 pre[i]为[0..i]里所有数的和，则 pre[i]可以由 pre[i−1]递推而来，即：

`pre[i]=pre[i−1]+nums[i]`

那么[j..i]这个子数组和为k这个条件我们可以转化为

`pre[i]−pre[j−1]==k`

简单移项可得符合条件的下标j需要满足:

`pre[j−1]==pre[i]−k`

所以我们考虑以i结尾的和为k的连续子数组个数时只要统计有多少个前缀和为`pre[i]−k`的`pre[j]`即可。

### 代码
``` java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0, pre = 0;
        HashMap < Integer, Integer > mp = new HashMap < > ();
        mp.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            pre += nums[i];
            if (mp.containsKey(pre - k)) {
                count += mp.get(pre - k);
            }
            mp.put(pre, mp.getOrDefault(pre, 0) + 1);
        }
        return count;
    }
}
```


# 2、Review
分享一篇如何用Python解决护士排班问题的文章：[Solving Nurse Scheduling/Rostering Problems in Python](https://medium.com/@muafirathasnikt/solving-nurse-scheduling-rostering-problems-in-python-d44acc3ed74f)


> One of the many problems associated with nursing staff management is the dreaded nursing scheduling problem. As a nurse manager, one must fill many positions at any given time while juggling the concerns and demands of your administrator and your staff. Creating a schedule week by week can be a huge headache and some people are bound to be disappointed. Even if meeting facility requirements is the priority, It is not allowed to overburden employees and push them to have overtime work.


# 3、Technique/Tips
对于个人隐私比较看重的朋友，推荐使用真正No Trace的搜索引擎：startpage.com  斯诺登都推荐过。

# 4、Share
Life is short. 人生真的苦短，做些自己想做的事，照顾好身边的人。别太玩命，到头来都是韭菜，没有必要。



