# 206-Reverse Linked List

## 题目描述

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

 

**Example 1:**

<img src="./206-Reverse_Linked_List.assets/rev1ex1.jpg" alt="img" />

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**

<img src="./206-Reverse_Linked_List.assets/rev1ex2.jpg" alt="img" />

```
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**

```
Input: head = []
Output: []
```

 

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

 

**Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?



## 题解V1.0 双指针

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
    public ListNode reverseList(ListNode head) {
        ListNode cur = head;
        ListNode pre = null;
        ListNode temp;
        while(cur != null){
            temp = cur.next;
            cur.next = pre;

            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
```

`python`

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        cur = head
        pre = None
        if not head:
            return
        while(cur):
            temp = cur.next #用temp来存储cur下一个指针的位置
            cur.next = pre #反向

            pre = cur
            cur = temp
        return pre
```

重点是有一个temp来存储cur下次要迭代的位置，因为cur.next已经翻转了，指到前面去了

## 题解V2.0 递归



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
    //递归法
    public ListNode reverseList(ListNode head) {
        return reverse(null, head);
    }
    public ListNode reverse(ListNode pre, ListNode cur){
        // 递归出口
        if (cur == null){
            return pre;
        }
        ListNode temp = cur.next;
        cur.next = pre;
        return reverse(cur, temp);
    }
}
```

这个与双指针法的思路一样。



## 题解V2.1 递归（逆向

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
    //递归法，从后往前
    public ListNode reverseList(ListNode head){
        if (head == null || head.next == null){
            return head;
        }
        //让head之后的链表翻转
        ListNode last = reverseList(head.next);
      	//处理head.next之前的（hea）
        head.next.next = head;
        head.next = null;
        
        return last;
    }
}
```

