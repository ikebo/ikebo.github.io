---
title: "ARTS打卡 第四周"
date: 2023-09-11T21:02:17+08:00
draft: false
---


# 1、Algorithm
## 无重复字符的最长子串
给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

### 示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

### 示例 2:

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

### 示例 3:

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

### 提示：

* 0 <= s.length <= 5 * 104
* s 由英文字母、数字、符号和空格组成


### 思路
这道题主要用到思路是：滑动窗口

什么是滑动窗口？

其实就是一个队列,比如例题中的 abcabcbb，进入这个队列（窗口）为 abc 满足题目要求，当再进入 a，队列变成了 abca，这时候不满足要求。所以，我们要移动这个队列！

如何移动？

我们只要把队列的左边的元素移出就行了，直到满足题目要求！

一直维持这样的队列，找出队列出现最长的长度时候，求出解！

时间复杂度：O(n)

### 代码
``` java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max = 0;
        int left = 0;
        for(int i = 0; i < s.length(); i ++){
            if(map.containsKey(s.charAt(i))){
                left = Math.max(left,map.get(s.charAt(i)) + 1);
            }
            map.put(s.charAt(i),i);
            max = Math.max(max,i-left+1);
        }
        return max;
        
    }
}
```


# 2、Review
分享一篇如何用React Native与Flutter的对比文章：[React Native vs. Flutter](https://blog.bitsrc.io/react-native-vs-flutter-a-quick-analysis-of-framework-benefits-for-app-development-in-2023-877c39e4cb5c)


> Flutter and React Native are the two most prominent players in the market for cross-platform app development. You may have an idea for an app but can be confused when deciding between Flutter and React Native.

Whether you are interested in React Native app development or Flutter app development, both have advantages and disadvantages, and the choice depends on whether you prefer native app development or cross-platform app development.


# 3、Technique/Tips
flutter 跨端能力太强了，个人感觉是未来的主流，有点像从Jquery时代来到了Vue.js时代，推荐使用。

# 4、Share
You are already naked, there is no reason not to follow your heart.

![steve jobs](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT8hmW_OoRsUxqoJH2QwCpG1ifU9UA0kju9xEvFRhU-5KF11UpofhHwBq5u8lxYUW-7a3k&usqp=CAU)

