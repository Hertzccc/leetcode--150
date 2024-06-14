# 66. Plus one

## 题目描述

You are given a **large integer** represented as an integer array `digits`, where each `digits[i]` is the `ith` digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading `0`'s.

Increment the large integer by one and return *the resulting array of digits*.

 

**Example 1:**

```
Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].
```

**Example 2:**

```
Input: digits = [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
Incrementing by one gives 4321 + 1 = 4322.
Thus, the result should be [4,3,2,2].
```

**Example 3:**

```
Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].
```

 

**Constraints:**

- `1 <= digits.length <= 100`
- `0 <= digits[i] <= 9`
- `digits` does not contain any leading `0`'s.



## 题解V1.0 判断9

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        for i in range(len(digits)-1, -1, -1):
            if(digits[i] != 9):
                digits[i] += 1
                for j in range(i+1, len(digits)):
                    digits[j] = 0
                return digits
        #都等于9
        return [1] + len(digits)*[0]

```

时间O(n)，即数组的长度

思路：从后向前遍历，找到第一个不为9的数字，将它加一，之后的变0.

这个数会有几种情况：

- digits末尾没有9：直接末尾+1
- digits末尾有9：从后向前，找到第一个不为9的，将其+1
- digits全是9：构造一个数组长度+1的新数组，首位变1，其他都是0



