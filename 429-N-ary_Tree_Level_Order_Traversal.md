# 429. N-ary Tree Level Order Traversal

## 题目描述

Given an n-ary tree, return the *level order* traversal of its nodes' values.

*Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).*

 

**Example 1:**

![img](./429-N-ary_Tree_Level_Order_Traversal.assets/narytreeexample.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: [[1],[3,2,4],[5,6]]
```

**Example 2:**

![img](./429-N-ary_Tree_Level_Order_Traversal.assets/sample_4_964.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: [[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
```

 

**Constraints:**

- The height of the n-ary tree is less than or equal to `1000`
- The total number of nodes is between `[0, 104]`



## 题解V1.0 Queue

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) { return result;}
        Queue<Node> queue = new ArrayDeque<>();
        
        queue.offer(root);//先压入头节点
        while(!queue.isEmpty()){
            int cur_size = queue.size(); // save the state of the current level size
            List<Integer> cur_level = new ArrayList<>(); // to add every current level to result

            for(int i=0; i<cur_size; i++){
                Node node = queue.poll();
                cur_level.add(node.val); //记录弹出的节点值
                for (Node child: node.children){
                    queue.offer(child);
                }
            }
            result.add(cur_level);
        }
        return result;
    }
}
```

与102不同的是，在31行的遍历每个节点的孩子，不像二叉树只要写if左右孩子。
