# 239. Sliding Window Maximum

## 题目描述

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return *the max sliding window*.

 

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`



## 题解V1.0 单调队列

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums.length == 0 || k == 0) return new int[0];
        Deque<Integer> bigger = new LinkedList<>();
        int[] res = new int[nums.length - k + 1];
				//
        int i; 
        for(int j=0; j < nums.length; j++){
            //若bigger中的头部最大值是就是滑框略过的元素nums[i-1]
            //剔除对应bigger中的值
            i = j-k+1;
            if(i>0 && !bigger.isEmpty() && bigger.peekFirst() == nums[i-1]){
                bigger.pollFirst();
            }
            //把小于新值的都排出去，保持bigger中的值递减，一定从后面开始遍历排啊
            while(!bigger.isEmpty() && bigger.peekLast() < nums[j]){
                bigger.pollLast();
            }   
            //新元素添加到后面(offerLast和offer一样)
            bigger.offer(nums[j]);
            if(i >= 0){ 
                res[i] = bigger.peekFirst();
            }
        }
        return res;
    }
}
```

时间O(N)

核心思路是：维护一个单调队列，队头是最大值。当滑框移动时，获取单调队列的最大值即队头，存到结果数组中。

如何维护？

在每次右滑框遍历找新值的过程中

- 设置左滑框的位置i
- 若左滑框滑过的值就是最大值，即不属于滑框的老值：
  - 将该老值从队列中剔除，`pollFirst()`。队列最大值就是队头，所以用`pollFirst()`
  - 思考：若滑过的值不是最大值？不关心，因为答案每次存入的都是滑框的最大值
- 持续将小于新值的都从队列中移除，为了保证队列队头是最大值且队列非严格递减，后面的是小值。从屁股处开始遍历。
  - 思考：为什么是屁股，不能从头么？不能。不能把大于它的剔除，所以只能从屁股处最小的开始
- 将新元素添加到队列后面。此时已经保证了，若新元素最大，那它是队头。若新元素不是最大，那它前面也没有比它小的。
- 最后记录窗口最大值，直接取队列头即可。

