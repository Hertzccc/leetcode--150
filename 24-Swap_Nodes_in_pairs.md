# 24. Swap Nodes in Pairs

## 题目描述

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

 

**Example 1:**

<img src="./24-Swap_Nodes_in_pairs.assets/swap_ex1.jpg" alt="img" />

```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

**Example 2:**

```
Input: head = []
Output: []
```

**Example 3:**

```
Input: head = [1]
Output: [1]
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`



## 题解V1.0 迭代

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(0,next=head)
        temp = dummy_head
        while(temp.next and temp.next.next):
            node1 = temp.next
            node2 = temp.next.next
            temp.next = node2 #交换
            node1.next = node2.next
            node2.next = node1
            #迭代
            temp = node1
        return dummy_head.next
```

时间O(N)，将temp, node1, node2 转换成 temp, node2, node1。每次迭代，temp都指向node1



题解V2.0 递归

`python`

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        #递归终止条件
        if not (head and head.next):
            return head
        temp = head.next
        head.next = self.swapPairs(temp.next)
        temp.next = head
        return temp 
```

`java`

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode temp = head.next;        //第二个节点temp
        head.next = swapPairs(temp.next); //第一个节点指向(3:)..
        temp.next = head;
        return temp;
    }
}
```



head    head.next  (递归)

Node1 Node2       (Node3....

递归出口：当前节点或下一个节点为空，返回

方法内容：当前节点next，指向当前节点，指针互换

​			也就是将1-> 下一层递归函数, 2->1，最后返回节点2

返回值：返回交换完成的节点（节点2）

每个递归体内都有一个head，因为我们传入了一个head节点。