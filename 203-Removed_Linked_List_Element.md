# 203. Remove Linked list element

## 题目描述

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return *the new head*.

 

**Example 1:**

![img](./203-Removed_Linked_List_Element.assets/removelinked-list.jpg)

```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```

**Example 2:**

```
Input: head = [], val = 1
Output: []
```

**Example 3:**

```
Input: head = [7,7,7,7], val = 7
Output: []
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 104]`.
- `1 <= Node.val <= 50`
- `0 <= val <= 50`



## 题解V1.0

`Java`

```java
// public class ListNode {
//     int val;
//     ListNode next;
//     ListNode() {}
//     ListNode(int val) { this.val = val; }
//     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
//     }
 
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) return head;
        ListNode new_head = new ListNode(-1, head);
        ListNode pre = new_head;
        ListNode cur = head;
        while(cur != null){
            if(cur.val == val){ pre.next = cur.next;}
            else {
                pre = cur;
            }
            cur = cur.next;
        }
        return new_head.next;
    }
}
```

构造一个新头 指向 旧头



```java
// public class ListNode {
//     int val;
//     ListNode next;
//     ListNode() {}
//     ListNode(int val) { this.val = val; }
//     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
//     }
 
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) return head;
        ListNode new_head = new ListNode(-1, head); //加一个新头
        ListNode cur = new_head; 										//cur指向新头
        while(cur.next != null){ //从老头开始遍历
            if(cur.next.val == val){ cur.next = cur.next.next;} 
            else {
                cur = cur.next;
            }
        }
        return new_head.next;
    }
}
```



`python`

```python
public ListNode removeElements(ListNode head, int val) {
    if (head == null) {
        return head;
    }
    // 因为删除可能涉及到头节点，所以设置dummy节点，统一操作
    ListNode dummy = new ListNode(-1, head);
    ListNode pre = dummy;
    ListNode cur = head;
    while (cur != null) {
        if (cur.val == val) {
            pre.next = cur.next;
        } else {
            pre = cur;
        }
        cur = cur.next;
    }
    return dummy.next;
}
```

