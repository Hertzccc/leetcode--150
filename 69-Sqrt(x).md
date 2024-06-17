# 69. Sqrt(x)

## 题目描述

Given a non-negative integer `x`, return *the square root of* `x` *rounded down to the nearest integer*. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

- For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.

 

**Example 1:**

```
Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.
```

**Example 2:**

```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.
```

 

**Constraints:**

- `0 <= x <= 231 - 1`



## 题解V1.0-暴力

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        for i in range(0, x//2+2):
            if(i*i == x):
                return i
            if(i*i >=x):
                return i - 1
```



## 题解V2.0-用e换底

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        if (x == 0):
            return 0
        ans = int(math.exp(0.5 * math.log(x)))
        return ans+1 if (ans+1) ** 2 <=x else ans
```

x^(1/2) = e^(1/2*lnx)，但是由于计算机存储浮点值会有偏差，最后还需要判断一下返回的结果



## 题解V3.0-二分查找（左闭右开）

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        l = 0
        r = x+1
        ans = -1

        while(l < r):
            mid = l + (r-l)//2
            if(mid * mid) <= x:
                l = mid + 1
                ans = mid
            else:
                r = mid
        return ans
```



## 题解V3.1-二分查找（左闭右闭）

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        l, r, ans = 0, x, -1

        while(l <= r):
            mid = l + (r-l)//2
            if(mid * mid) <= x:
                l = mid + 1
                ans = mid
            else:
                r = mid - 1
        return ans
```

