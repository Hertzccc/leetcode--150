# 107. Binary Tree Level Order Traversal II

## 题目描述

Given the `root` of a binary tree, return *the bottom-up level order traversal of its nodes' values*. (i.e., from left to right, level by level from leaf to root).

 

**Example 1:**

![img](./107-Binary_Tree_Level_Order_Traversal_II.assets/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[15,7],[9,20],[3]]
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



## 题解V1.0 queue迭代

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> result = new LinkedList<>();//适合从头部插入元素
        if(root == null){ return result;}
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int cur_size = queue.size();//获取当前层的节点个数
            List<Integer> cur_level = new LinkedList<>();
            for(int i=0; i<cur_size; i++){
                TreeNode node = queue.poll(); //弹出queue顶元素
                cur_level.add(node.val);
                //添加左右元素
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }//遍历完后表示此层结束，需要添加到结果中
            //与102不同的是，需要从头开始添加元素
            result.add(0,cur_level);
        }
        return result;
    }
}
```

和102不同的只是选用了LinkedList，因为最后加入result一直是从头节点加入。

