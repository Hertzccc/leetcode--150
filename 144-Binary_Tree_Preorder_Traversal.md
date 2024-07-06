# 144. Binary Tree Preorder Traversal



## 题目描述

Given the `root` of a binary tree, return *the preorder traversal of its nodes' values*.

 

**Example 1:**

![img](./144-Binary_Tree_Preorder_Traversal.assets/inorder_1.jpg)

```
Input: root = [1,null,2,3]
Output: [1,2,3]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [1]
Output: [1]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

 

**Follow up:** Recursive solution is trivial, could you do it iteratively?

## 题解V1.0 递归

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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorder(root,result);
        return result;
    }
    //define the parameters
    public void preorder(TreeNode node, List<Integer> result){
        // confirm the recursion base case
        if(node == null){
            return;
        }
        //root
        result.add(node.val);
        //left and right
        preorder(node.left, result);
        preorder(node.right, result);
    }
}
```

前序遍历就是preorder，order是针对头节点说的，先遍历头节点。



## 题解V2.0 迭代

```java

class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        //initialization
        List<Integer> result = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        if (root == null){ return result;}
        //主循环
        TreeNode cur = root;
        while (cur != null || stack.peek() != null){
            //一直遍历，将中，左全部放进去
            while(cur != null){
                result.add(cur.val);
                stack.push(cur);
                cur = cur.left;
            }//处理右节点，右节点设置为cur
            cur = stack.pop();
            cur = cur.right;
        }
        return result;
    }
}
```

有个重要的概念：遍历节点和处理节点。对于这里来说，遍历的节点和处理的节点是一样的。

从头开始一直向左，边遍历边加入result边压入stack，所以在之后处理右节点时，也是从左下开始一直向右（stack弹出对应的右节点的头节点）