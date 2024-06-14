# 35- Search Index position

## 题目描述

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [1,3,5,6], target = 5
Output: 2
```

**Example 2:**

```
Input: nums = [1,3,5,6], target = 2
Output: 1
```

**Example 3:**

```
Input: nums = [1,3,5,6], target = 7
Output: 4
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` contains **distinct** values sorted in **ascending** order.
- `-104 <= target <= 104`



## 题解V1.0-二分

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        l, r, ans = 0, len(nums)-1, -1
        while(l <= r):
            #target==mid的话，要返回等于的那个值
            #target > mid的话，到右边找
            #target < mid的话，到左边找
            #若出了循环，位置就是l==r的时候
            mid = (l+r) // 2
            if(target > nums[mid]):
                l = mid + 1
            elif(target < nums[mid]):
                r = mid - 1
            else:
                return mid
        return l

```

