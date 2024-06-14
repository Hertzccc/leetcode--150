# 9. Palindrome number

## 题目描述

Given an integer `x`, return `true` *if* `x` *is a*  ***palindrome*** *, and* `false` *otherwise*.

 

**Example 1:**

```
Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
```

**Example 2:**

```
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```



## 题解V1.0

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if(x < 0):
            return False
        remainders = []
        while(x > 0):
            r = x % 10
            remainders.append(r)
            x = x //10
        for i in range(len(remainders)):
            if(remainders[i] != remainders[len(remainders)-1 - i]):
                return False
        return True 
```

将回文数每个数字储存到数组，遍历比较，O(n)，其实在取模的过程只有O(logn)。所以另一个思路是翻转数字



## 题解V2.0

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if(x < 0) or (x % 10 == 0 and x != 0):
            return False
        reverse = 0
        num = x
        while(x > 0):
            r = x % 10
            x = x //10
            reverse = reverse * 10 + r
        return num == reverse
```

及时求出回文数，最后比较。时间是O(logn)，还可以优化为只模一半



## 题解V3.0

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if(x < 0) or (x % 10 == 0 and x != 0):
            return False
        reverse_half = 0
        while(x > reverse_half):
            r = x % 10
            x = x //10
            reverse_half = reverse_half * 10 + r
        return reverse_half == x or reverse_half // 10 == x
```

时间依然是O(logn)，不过只要计算一半的值