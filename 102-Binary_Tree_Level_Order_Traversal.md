# 102. Binary Tree Level Order Traversal

## 题目描述

Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

 

**Example 1:**

<img src="./102-Binary_Tree_Level_Order_Traversal.assets/tree1.jpg" alt="img" />

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

**Example 2:**

```
Input: root = [1]
Output: [[1]]
```

**Example 3:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`



## 题解V1.0 用Queue

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) { return result;}
        Queue<TreeNode> queue = new ArrayDeque<>();
        
        queue.offer(root);//先压入头节点
        while(!queue.isEmpty()){
            int cur_size = queue.size(); // save the state of the current level size
            List<Integer> cur_level = new ArrayList<>(); // to add every current level to result

            for(int i=0; i<cur_size; i++){
                TreeNode node = queue.poll();
                cur_level.add(node.val); //记录弹出的节点值
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }
            result.add(cur_level);
        }
        return result;
    }
}
```

先压入头节点，我们将每层的节点数储存在queue中。这个存储方式如何实现？

当每层的节点完全出queue后，遗留在queue中的节点数量就是下一层的数量。在下次循环获取queue的size时，就固定了内循环所需的次数，即弹出多少个节点位于该层。每次弹出节点，都会offer它的左右孩子进queue。

每层对应的cur_level添加进result，即每层对应的节点值。