# 110. Balanced Binary Tree

## 题目描述

Given a binary tree, determine if it is 

**height-balanced**

.



 

**Example 1:**

![img](./110-Balanced_Binary_Tree.assets/balance_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

**Example 2:**

![img](./110-Balanced_Binary_Tree.assets/balance_2.jpg)

```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```

**Example 3:**

```
Input: root = []
Output: true
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-104 <= Node.val <= 104`



## 题解V1.0 后序遍历，返回高度

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
    //采用后序遍历，左右中
    public boolean isBalanced(TreeNode root) {
        return getHeight(root) != -1;
    }
    //新加一个方法用于递归高度，若已经不是平衡二叉树，则返回-1
    public int getHeight(TreeNode root){
        if(root == null){
            return 0; //注意是0，如果是最后一行的子节点，高度应为0
        }
        int leftHeight = getHeight(root.left);      //左
        int rightHeight = getHeight(root.right);    //右

        if(Math.abs(leftHeight - rightHeight) > 1){ //不是平衡二叉树
            return -1;
        }
        int result = Math.max(leftHeight, rightHeight)+1;//中
        return result;
    }
}
```

