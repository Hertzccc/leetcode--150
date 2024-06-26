# 面试题02.07. Intersection of 2 linked Lists LCCI

## 题目描述

Given two (singly) linked lists, determine if the two lists intersect. Return the inter­ secting node. Note that the intersection is defined based on reference, not value. That is, if the kth node of the first linked list is the exact same node (by reference) as the jth node of the second linked list, then they are intersecting.

**Example 1:**

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
Output: Reference of the node with value = 8
Input Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,0,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```

**Example 2:**

```
Input: intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Reference of the node with value = 2
Input Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [0,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```

**Example 3:**

```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: null
Input Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```

**Notes:**

- If the two linked lists have no intersection at all, return `null`.
- The linked lists must retain their original structure after the function returns.
- You may assume there are no cycles anywhere in the entire linked structure.
- Your code should preferably run in O(n) time and use only O(1) memory.



## 题解V1.0 双指针

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        #分别遍历获取两个长度
        len_A, len_B = 0, 0
        cur = headA
        while(cur):
            len_A +=1
            cur = cur.next
        cur = headB
        while(cur):
            len_B += 1
            cur = cur.next

        cur_A, cur_B = headA, headB
        if(len_B < len_A): #设置B是大的那个
            len_A, len_B = len_B, len_A
            cur_A, cur_B = cur_B, cur_A
        for _ in range(len_B-len_A):
            cur_B = cur_B.next
        for _ in range(len_A):
            if(cur_A == cur_B):
                return cur_A
            else:
                cur_A = cur_A.next
                cur_B = cur_B.next
        return None
```

时间O(m+n)先各遍历一遍，获得A和B的长度。选短的那条开始遍历，长的末端对齐。双指针是分别指向A和B的。

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        #若有交点，A+B和B+A的尾部一定有交点
        if(not headA or not headB):
            return None
        curA, curB = headA, headB
        while(curA != curB):
            curA = curA.next if curA != None else headB
            curB = curB.next if curB != None else headA
        return curA
```



