# 977. Squares of a Sorted Array

## 题目描述

Given an integer array `nums` sorted in **non-decreasing** order, return *an array of **the squares of each number** sorted in non-decreasing order*.

 

**Example 1:**

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

**Example 2:**

```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in **non-decreasing** order.

 

**Follow up:** Squaring each element and sorting the new array is very trivial, could you find an `O(n)` solution using a different approach?



## 题解V1.0，双指针头尾

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        i, j = 0, len(nums)-1
        pos = len(nums)-1
        nums2 = [0]*len(nums)
        while(i <= j):
            s1 = nums[i] * nums[i]
            s2 = nums[j] * nums[j]
            if(s1 > s2):
                nums2[pos] = s1
                i += 1
            else:
                nums2[pos] = s2
                j -= 1
            pos -= 1
        return nums2
```

若所有nums中的数都平方，应该是漏斗状的，两边大中间小

时间复杂度就是 O(N)，只遍历了一次数组。新建一个长度相等的数组，将最两端较大值放到新数组中。出循环条件应是i>j，因为相等时还未放入新数组