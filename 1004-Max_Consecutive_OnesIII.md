# 1004. Max Consecutive OnesIII

## 题目描述

Given a binary array `nums` and an integer `k`, return *the maximum number of consecutive* `1`*'s in the array if you can flip at most* `k` `0`'s.

 

**Example 1:**

```
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

**Example 2:**

```
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.
- `0 <= k <= nums.length`

## 题解V1.0 最大滑动窗口

```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        max_ = i = 0
        #最大滑动窗口
        for j in range(len(nums)):
            if nums[j] == 0:
                k -= 1
            while(i <= j and k < 0): #这里试了很多次，需要保证i可能会和j重合
                if nums[i] == 0:
                    k += 1
                i += 1
            max_ = max_ if max_ > j-i+1 else j-i+1
        return max_
            
```

时间O(N)，右窗口持续移动，遇到0的话，k--。当k减到-1，开始收缩左窗口，直到左窗口出现一个0，i++,相当于释放了0的位置。
