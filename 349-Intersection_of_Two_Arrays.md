# 349. Intersection of 2 arrays

## 题目描述

Given two integer arrays `nums1` and `nums2`, return *an array of their* 

*intersection*

. Each element in the result must be **unique** and you may return the result in **any order**.



 

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```



## 题解V1.0 哈希

##### `Counter()`

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        nums1_hash = Counter(nums1)
        num = []
        for x in nums2:
            if x in nums1_hash.keys() and x not in num:
                num.append(x)
        return num
```

​	

`Set()`

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        nums1_hash = set(nums1)
        num = set()
        for x in nums2:
            if x in nums1:
                num.add(x)
        
        return list(num)
```

第二种更好，Java中用HashSet即可

`java`

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> nums = new HashSet<>();
        for(int x : nums1){
            set1.add(x);
        }
        for(int x : nums2){
            if(set1.contains(x)){
                nums.add(x);
            }
        }
       return nums.stream().mapToInt(Integer::valueOf).toArray();
    }
}
```

时间`O(M+N) `，M是nums1的长度，N是nums2的长度

`nums.stream()`:将nums转换成流

`mapToInt()`: 接受一个方法的引用，这里是`Integer::valueOf`，这个方法将`Integer`转换成基本类型`int`，

