# 19. Remove Nth Node From End of List

## 题目描述

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

 

**Example 1:**

<img src="./19-Remove_Nth_Node_From_End.assets/remove_ex1.jpg" alt="img" />

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

 

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`



## 题解V1.0 遍历两次+添加dummy_head

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy_head = ListNode(0,next=head)
        cur = dummy_head
        length = 0
        while(cur.next):
            length += 1 
            cur = cur.next
        if n > length: return []
        cur = dummy_head
        for i in range(length-n):
            cur = cur.next
        cur.next = cur.next.next
        return dummy_head.next
```

时间O(L), L即链表长度。先遍历一遍确定链表长度，然后再从head遍历找到要删除节点的**前一个节点**。

若要仅遍历一次
