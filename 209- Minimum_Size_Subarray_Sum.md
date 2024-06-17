# 209. Minimum Size Subarray Sum

## 题目描述

Given an array of positive integers `nums` and a positive integer `target`, return *the **minimal length** of a* 

*subarray*

 *whose sum is greater than or equal to* `target`. If there is no such subarray, return `0` instead.

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

**Example 2:**

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

**Example 3:**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

 

**Constraints:**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

 

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.



## 题解V1.0 滑动窗口

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        #滑动窗口，在窗口中是>=target的值
        #i：滑动起始位置
        #j：滑动终止位置
        #更新i：窗口内值大了
        #更新j：窗口内值小了
        i, j = 0, 0
        min_ = float('inf') # 无穷大 min_是窗口大小
        while(j < len(nums)):
            sum_ = 0
            length = j - i + 1#此时窗口大小
            for x in range(i, j+1): #计算窗口内值
                sum_ += nums[x]
            if sum_ < target and j < len(nums):
                j = j + 1
            elif sum_ >= target: 
                min_ = length if length <= min_ else min_
                i += 1
            else: #j到头了 sum依然小于 target
                break
            
        if min_ == float('inf'):
            return 0
        else:
            return min_
```

时间O(n^2)，想写成O(n)的滑动窗口，但是写成了O(n^2)



## 题解V1.1 滑动窗口更新

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        #滑动窗口，在窗口中是>=target的值
        #i：滑动起始位置
        #j：滑动终止位置
        #更新i：窗口内值大了
        #更新j：持续更新，窗口内值不够大（里面的i会持续右移来保持窗口内值<target)
        i, j = 0, 0
        min_ = float('inf') # 无穷大 min_是窗口大小
        sum_ = 0 #累积值，在更新i时会减去前面得值
        while(j < len(nums)):
            sum_ += nums[j]

            while(sum_ >= target):
                min_ = min_ if min_ < j-i+1 else j-i+1 #更新窗口大小
                sum_ -= nums[i]
                i += 1
            j += 1
        return 0 if min_ == float('inf') else min_
```

此时时间复杂度是O(N)，不需要while循环来计算sum。

选择持续更新滑动终止位置j，每次更新j前，都保证了滑动窗口内的值小于target。一旦i超过j的时刻，sum_值也就清零了，循环结束时，j得到更新向右滑动，又保证了滑动窗口有新值。其实当i超过j时，答案已经明确是1，已经有一个值>=target了。

