# 202. Happy Number

## 题目描述

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process **ends in 1** are happy.

Return `true` *if* `n` *is a happy number, and* `false` *if not*.

 

**Example 1:**

```
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

**Example 2:**

```
Input: n = 2
Output: false
```

 

**Constraints:**

- `1 <= n <= 2^31 - 1`



## 题解V1.0 用哈希集合Set来存储可能存在的环值

```python
class Solution:
    #给定一个数，它的下一个数会是什么
    def getNext(self, num:int) -> int:
        sum_ = 0
        while(num > 0):
            digit = num % 10
            sum_ += digit ** 2
            num //= 10
        return sum_
    def isHappy(self, n: int) -> bool:
        loop = set()
        while(n != 1 and n not in loop):
            loop.add(n)
            n = self.getNext(n)
        return n == 1
```

设置一个set，每次将next数塞进去，若出现相同的，则不快乐，直到变成1

时间O(logN)



## 题解V2.0快慢指针（弗洛伊德循环查找

```python
class Solution:
    #给定一个数，它的下一个数会是什么
    def isHappy(self, n: int) -> bool:
        def getNext(num:int) -> int:
            sum_ = 0
            while(num > 0): #每次从后往前获取一位，累加
                digit = num % 10
                sum_ += digit ** 2
                num //= 10
            return sum_

        slow = n
        fast = getNext(n)
        #用检测环的方法
        while(fast != 1 and fast != slow):
            slow = getNext(slow)
            fast = getNext(getNext(fast))
        return fast == 1
```

开始不能让快慢指针都位于起点，要不然进不了循环（毕竟fast=slow=n在初始化时）