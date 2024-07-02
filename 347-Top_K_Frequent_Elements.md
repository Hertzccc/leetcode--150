# 347. Top K Frequent Elements

## 题目描述

Given an integer array `nums` and an integer `k`, return *the* `k` *most frequent elements*. You may return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `k` is in the range `[1, the number of unique elements in the array]`.
- It is **guaranteed** that the answer is **unique**.

 

**Follow up:** Your algorithm's time complexity must be better than `O(n log n)`, where n is the array's size.



## 题解V1.0

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
    //思路
    //统计频率用hashmap
    //频率排序用PriorityQueue
    //返回频率
        //初始化
        Map<Integer, Integer> frequency = new HashMap<>();
        PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>(
            (o1, o2) -> 
                o2.getValue() - o1.getValue()
            );
        int[] ans = new int[k];
        //遍历数组
        for(int num: nums){
            frequency.put(num, frequency.getOrDefault(num, 0) + 1);
        }
        //添加每个entry
        for(Map.Entry<Integer, Integer> fre: frequency.entrySet()){
            pq.add(fre);
        }
        for(int i=0; i < k; i++){
            if(!pq.isEmpty()){
                ans[i] = pq.poll().getKey();
            }
        }
        return ans;
    }
}
```

时间O(Nlog(k))

- 先统计频率
- 频率排序
- 返回k个最大的频率

