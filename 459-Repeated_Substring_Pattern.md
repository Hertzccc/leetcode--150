# 459. Repeated Substring Pattern

## 题目描述

Given a string `s`, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

 

**Example 1:**

```
Input: s = "abab"
Output: true
Explanation: It is the substring "ab" twice.
```

**Example 2:**

```
Input: s = "aba"
Output: false
```

**Example 3:**

```
Input: s = "abcabcabcabc"
Output: true
Explanation: It is the substring "abc" four times or the substring "abcabc" twice.
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of lowercase English letters.

## 题解V1.0 KMP+移动匹配

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        next_ = self.getNext(s)
        q = -1
        ss = s[1:]+s[:-1]
        for i in range(len(ss)):
            while(q >= 0 and ss[i] != s[q+1]):
                q = next_[q]
            if(ss[i] == s[q+1]):
                q += 1
            if(q == len(s)-1):
                return True
        return False

#KMP的next数组获取
    def getNext(self, s:str) -> list[int]:
        #intialization
        q = -1
        next_ = [-1 for _ in range(len(s))]
        for i in range(1, len(s)):
            while(q >= 0 and s[i] != s[q+1]):
                q = next_[q]
            if s[i] == s[q+1]:
                q += 1
            next_[i] = q
        return next_
```

时间O(N)，先求出s的next数组。

拼接ss = s[1:]+s[:-1]，若中间有匹配到s，那说明有重复的子串能构成s。



## 题解V2.0 KMP优化

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        next_ = self.getNext(s)
        q = -1
        # 原字符串长度-最长相等前后缀的长度，若能被len(s)整除
        #还要保证最后一个不是-1，即最长相等前后缀长度是0的话，len(s)%len(s)也是可以整除的
        return next_[len(s)-1] != -1 and len(s) % (len(s) - (next_[len(s)-1]+1)) == 0
            

    def getNext(self, s:str) -> list[int]:
        #intialization
        q = -1
        next_ = [-1 for _ in range(len(s))]
        for i in range(1, len(s)):
            while(q >= 0 and s[i] != s[q+1]):
                q = next_[q]
            if s[i] == s[q+1]:
                q += 1
            next_[i] = q
        return next_
```

如果我们设 iii 为最小的起始位置，那么一定有 $gcd(n,i)=i$，即 `n` 是 `i` 的倍数。这说明字符串 `s` 是由长度为 `i` 的前缀重复 $\frac{n}{i} $ 次构成；

**由于 $next[n−1] $表示 `s` 具有长度为 $next[n−1]+1$ 的完全相同的（且最长的）前缀和后缀**。那么对于满足题目要求的字符串，一定有$ next[n−1]=n−i−1$，即 $i=n−next[n−1]−1$；

对于不满足题目要求的字符串，$n$ 一定不是 $n−next[n−1]−1$ 的倍数。

