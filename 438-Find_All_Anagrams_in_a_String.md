# 438. Find All Anagrams in a String

## 题目描述

Given two strings `s` and `p`, return *an array of all the start indices of* `p`*'s anagrams in* `s`. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

**Example 1:**

```
Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```
Input: s = "abab", p = "ab"
Output: [0,1,2]
Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

 

**Constraints:**

- `1 <= s.length, p.length <= 3 * 104`
- `s` and `p` consist of lowercase English letters.



## 题解V1.0 两个counter 滑动窗口

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        hash_ = Counter()#用于计数
        ori_ = Counter(p) #origin 用于对比
        i = 0
        ans = []
        for j in range(len(s)):
            hash_[s[j]] += 1
            #若窗口大于p的长度，
            if(i <= j and j-i+1>len(p)):
                hash_[s[i]] -= 1
                i+=1
            #如果和原始一样
            if ori_ == hash_:
                ans.append(i)

        return ans
```

时间O(N)，

一个hashmap用来计数，另一个用来保存原始的数量用来做对比。每次滑动窗口右边界右移，计数的hashmap++，

## 题解V2.0 两个Counter换成数组

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        if len(p) > len(s): return []
        #初始化
        hash_ = [0]*26
        ori_ = [0]*26
        ans = []
        #记录ori存储的值
        for i in range(len(p)):
            ori_[ord(p[i]) - ord('a')] += 1
        i = 0
        for j in range(len(s)):
            hash_[ord(s[j]) - ord('a')] += 1
            #若计数大于原始
            if(i <= j and j-i+1>len(p)):
                hash_[ord(s[i]) - ord('a')] -= 1
                i+=1
            #如果和原始一样
            if ori_ == hash_:
                ans.append(i)
        return ans
```

速度变快很多
