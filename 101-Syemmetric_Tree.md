# 101. Symmetric Tree

## 题目描述

Given the `root` of a binary tree, *check whether it is a mirror of itself* (i.e., symmetric around its center).

 

**Example 1:**

![img](./101-Syemmetric_Tree.assets/symtree1.jpg)

```
Input: root = [1,2,2,3,4,4,3]
Output: true
```

**Example 2:**

![img](./101-Syemmetric_Tree.assets/symtree2.jpg)

```
Input: root = [1,2,2,null,3,null,3]
Output: false
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `-100 <= Node.val <= 100`

 

**Follow up:** Could you solve it both recursively and iteratively?



## 题解V1.0 用stack来储存相对的值

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
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        Deque<TreeNode> stack =  new LinkedList<>();
            stack.offerFirst(root.left);
            stack.offerFirst(root.right);        
        while(!stack.isEmpty()){
            TreeNode rightNode = stack.pollFirst();
            TreeNode leftNode = stack.pollFirst();
            if(rightNode == null && leftNode == null){ continue;}
            if(rightNode == null || leftNode == null || leftNode.val != rightNode.val){
                return false;
            }
            stack.offerFirst(leftNode.left);
            stack.offerFirst(rightNode.right);
            stack.offerFirst(leftNode.right);
            stack.offerFirst(rightNode.left);
        }
        return true;
    }
}
```

注意，这里用了offerFirst()来代替push()，用了pollFirst()来代替pop()，因为pop()会抛出空指针异常，而pollFirst()会返回null