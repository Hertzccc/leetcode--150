# 283 Move zeros

## 题目描述

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

 

**Example 1:**

```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Example 2:**

```
Input: nums = [0]
Output: [0]
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`



## 题解V1.0 快慢指针

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i = 0 #fast pointer
        j = 0 #slow pointer
        count = 0 # count of the occurence of 0
        while(i < len(nums)):
            if (nums[i] != 0):
                nums[j] = nums[i]
                j += 1
            else:
                count += 1
            i += 1
        nums[len(nums)-count:] = [0]*count
        return nums
```

时间复杂度O(N)，还是快慢指针。一个count用来记录0的个数