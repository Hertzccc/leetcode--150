# 111. Minimum Depth of Binary Tree

## 题目描述

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

 

**Example 1:**

![img](./111-Minimum_Depth_of_Binary_Tree.assets/ex_depth.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```

**Example 2:**

```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 105]`.
- `-1000 <= Node.val <= 1000`



## 题解V1.0 递归前序遍历 preorder

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
    public int minDepth(TreeNode root) {
        //recursion 后序遍历
        if(root == null) { return 0;}
        int left_depth = minDepth(root.left);     //左
        int right_depth = minDepth(root.right);   //右
        if(root.left == null && root.right != null){
            return 1 + right_depth;
        }
        if(root.right == null && root.left != null){
            return 1 + left_depth;
        }

        return Math.min(left_depth, right_depth)+1;

    }
}
```



## 题解V2.0 层序遍历

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
    public int minDepth(TreeNode root) {
        //层序遍历
        if(root == null) {return 0;}
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;
        while(!queue.isEmpty()){
            int length = queue.size();
            depth++;
            for(int i=0; i<length; i++){
                TreeNode node = queue.poll();
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
                if(node.left == null && node.right == null){
                    return depth;
                }
            }
        }
        return depth;
    }
}
```

