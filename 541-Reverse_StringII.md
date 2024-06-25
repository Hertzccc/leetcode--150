# 541. Reverse String II

## 题目描述

Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and leave the other as original.

 

**Example 1:**

```
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
```

**Example 2:**

```
Input: s = "abcd", k = 2
Output: "bacd"
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of only lowercase English letters.
- `1 <= k <= 104`

## 题解V1.0 转化成数组

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        #左闭右开
        def reverse(start:int, end:int) -> str:
            for i in range((end-start)//2):
                s[i+start], s[end-1 - i] = s[end-1 - i], s[i+start]
        s = list(s)
        for j in range(0, len(s), 2*k):
            reverse(j, min(j+k, len(s)))
        
        return "".join(s)
```

时间O(N)



## 题解V2.0 切片

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        #字符串分为0:i, i:i+k, i+k:
        for i in range(0, len(s), 2*k):
            s = s[0:i] + s[i:i+k][::-1] + s[i+k:] #python中不用担心字符串切片越界问题
        return s
```

时间O(N)，在切片中，如果右边界>长度，会切到右边界而不会越界。

