# 34. Find First and Last Position of Element in Sorted Array

## 题目描述

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

**Example 3:**

```
Input: nums = [], target = 0
Output: [-1,-1]
```

 

**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` is a non-decreasing array.
- `-109 <= target <= 109`



## 题解V1.0（失败）

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        l, r = 0, len(nums)-1
        while(l <= r):
            mid = (l + r) // 2
            if(target > nums[mid]):
                l = mid + 1
            elif(target < nums[mid]):
                r = mid - 1
            else:
                left,right = mid,mid
                #left
                while(left > 0):
                    left -= 1
                    if(nums[left] != target):
                        left += 1
                        break
                    
                while(right < len(nums)-1):
                    right += 1
                    if(nums[right] != target):
                        right -= 1
                        break
                return [left, right] 
        return [-1, -1]
        if not nums:
            return[-1, -1]
        #先二分找到target，再左右找到边界值
        
```

时间复杂度O(logn + n)，不满足题目要求logn



## 题解V2.0 两个二分

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if (not nums):
                return[-1, -1]
        left = self.leftBorder(nums, target)
        right = self.rightBorder(nums, target)
        if(left == -2 or right == -2):
            return [-1, -1]
        else:
            return [left, right]
                    

        #左边界
    def leftBorder(self, nums:List[int], target:int) -> int:
        l, r = 0, len(nums)-1
        left = -2
        while(l <= r):
            mid = (l + r) // 2
            if(target > nums[mid]):
                l = mid + 1
            elif(target < nums[mid]):
                r = mid - 1
            else:
                r = mid - 1
                left = r + 1
        return left

    def rightBorder(self, nums:List[int], target:int) -> int:
        l, r = 0, len(nums)-1
        right = -2
        while(l <= r):
            mid = (l + r) // 2
            if(target > nums[mid]):
                l = mid + 1
            elif(target < nums[mid]):
                r = mid - 1
            else:
                l = mid + 1
                right = l - 1
        return right
```

先用两个二分分别找左右边界，然后集中判断。因为提前设置了left=right=-2,



## 题解V2.1 删除冗余判断

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if (not nums):
                return[-1, -1]
        return [self.leftBorder(nums, target), self.rightBorder(nums, target)]
                    

        #左边界
    def leftBorder(self, nums:List[int], target:int) -> int:
        l, r = 0, len(nums)-1
        left = -1
        while(l <= r):
            mid = (l + r) // 2
            if(target > nums[mid]):
                l = mid + 1
            elif(target < nums[mid]):
                r = mid - 1
            else:
                left = mid
                r = mid - 1              
        return left

    def rightBorder(self, nums:List[int], target:int) -> int:
        l, r = 0, len(nums)-1
        right = -1
        while(l <= r):
            mid = (l + r) // 2
            if(target > nums[mid]):
                l = mid + 1
            elif(target < nums[mid]):
                r = mid - 1
            else:
                right = mid
                l = mid + 1

        return right
```



## 题解V2.2 将else情况移入if条件内（等于的情况并入）

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if (not nums):
                return[-1, -1]
        left = self.leftBorder(nums, target)
        right = self.rightBorder(nums, target)
        if left > right or left == -1 or right == -1:
            return [-1, -1]
        return [left, right]
                    

        #左边界
    def leftBorder(self, nums:List[int], target:int) -> int:
        l, r = 0, len(nums)-1
        left = -1
        while(l <= r):
            mid = (l + r) // 2
            if(target > nums[mid]):
                l = mid + 1
            elif(target <= nums[mid]):
                left = mid
                r = mid - 1
        return left

    def rightBorder(self, nums:List[int], target:int) -> int:
        l, r = 0, len(nums)-1
        right = -1
        while(l <= r):
            mid = (l + r) // 2
            if(target >= nums[mid]):
                right = mid
                l = mid + 1
            elif(target < nums[mid]):
                r = mid - 1
        return right
```

两个函数很相似，可以尝试合并到一个函数

## 题解V2.3 加入一个bool参数，合并成一个函数

```python3
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if (not nums):
                return[-1, -1]
        #第三个参数是isLeft，确定左右边界
        left = self.findBorder(nums, target, True)
        right = self.findBorder(nums, target, False)
        if left == -1 or right == -1: # target根本就不在nums数组中
            return [-1, -1]
        return [left, right]
    
    def findBorder(self, nums:List[int], target:int, isLeft:bool) -> int:
        l, r = 0, len(nums)-1
        border = -1
        while(l <= r):
            mid = (l + r) // 2
            if(target > nums[mid]):
                l = mid + 1
            elif(target < nums[mid]):
                r = mid - 1
            else:
                border = mid # 在target=nums[mid]中，决定边界如何更新
                if(isLeft):
                    r = mid - 1
                else:
                    l = mid + 1
        return border
```

