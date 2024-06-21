# 142. Linked List Cycle II

## 题目描述

Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

 

**Example 1:**

<img src="./142-Linked_List_CycleII.assets/circularlinkedlist.png" alt="img" />

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

**Example 2:**

<img src="./142-Linked_List_CycleII.assets/circularlinkedlist_test2.png" alt="img" />

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

**Example 3:**

<img src="./142-Linked_List_CycleII.assets/circularlinkedlist_test3.png" alt="img" />

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

 

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.



## 题解V1.0 哈希表

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        visited = {}
        cur = head

        while(cur):
            if cur in visited:
                return cur
            else:
                visited[cur] = True
            cur = cur.next
        return None
```

时间、空间都是O(N)，简单把节点的引用存到哈希。



## 题解V2.0 Floyd's Tortoise and Hare algorithm(cycle detection Alg)

设置`slow`和`fast`指针，fast`是` `slow`的两倍快，当他们相遇时

设从链表头到环起点距离为`L`，环的起始点到相遇点为`x`，环的长度为`c`。

`slow`走了 `L+x`, `fast`走了`L+x+kc(k>0)`，`fast`走过的节点数其实是`slow`走过节点数的2倍，所以有:

`L+x+kc=2*(L+x)`,-> `kc=L+x`. 若k = 1，在这里可以看出来，若`fast`在环里转一圈c，就碰到了`slow(L+x)`。若k > 1，也就是fast走了n圈，碰到了刚出门的slow，相遇点是环入口。**注意：此时fast速度是slow的速度**

**所以只需要在`fast与slow`相遇之后，设置`slow`从起点出发，再次碰到就是环入口**



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = fast = head

        while(fast and fast.next):
            fast = fast.next.next
            slow = slow.next
            if slow == fast:
                break
        if not(fast and fast.next):
            return None
        slow = head
        while(slow != fast):
            fast = fast.next
            slow = slow.next
        return slow
```

时间是O(N), 空间是O(1)

