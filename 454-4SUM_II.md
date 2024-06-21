# 454. 4SUM II

## 题目描述

Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length `n`, return the number of tuples `(i, j, k, l)` such that:

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

 

**Example 1:**

```
Input: nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
Output: 2
Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**Example 2:**

```
Input: nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
Output: 1
```

 

**Constraints:**

- `n == nums1.length`
- `n == nums2.length`
- `n == nums3.length`
- `n == nums4.length`
- `1 <= n <= 200`
- `-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228`



## 题解V1.0 哈希+分治

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        # nums1_2 = Counter() 
        #key：sum(nums1[x],nums2[y])，
        #value：sum(nums1[x],nums2[y])出现的次数
        # for x in nums1:
        #     for y in nums2:
        #         nums1_2[x+y] += 1
        nums1_2 = Counter(x+y for x in nums1 for y in nums2)
        count = 0
        for x in nums3:
            for y in nums4:
                if -(x+y) in nums1_2:
                    n = x+y
                    count += nums1_2[-x-y]
        return count
```

O(n^2)

将4数组分为2和2，若分为1,3，3的那份会造成O(n^3)，

键值对:

- key: sum(nums1[x],nums2[y])，存储第一个和第二个数组的所有元素加和
- value: sum(nums1[x],nums2[y])出现的次数，存储这些加和出现的次数

