# 94. Binary Tree Inorder Traversal



## 题目描述

Given the `root` of a binary tree, return *the inorder traversal of its nodes' values*.

 

**Example 1:**

![img](./94-Binary_Tree_Inorder_Traversal.assets/inorder_1-20240705113050982.jpg)

```
Input: root = [1,null,2,3]
Output: [1,3,2]
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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorder(root, result);
        return result;
    }
    public void inorder(TreeNode node, List<Integer> result){
        if(node == null){ return;}
        inorder(node.left, result);
        result.add(node.val);
        inorder(node.right, result);
    }

}
```

## 题解V2.0 迭代

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        if(root == null){ return result;}

        TreeNode cur = root;
        while(cur!=null || !stack.isEmpty()){
            while(cur!=null){
                stack.push(cur);
                cur = cur.left;
            }
            //这时是最左下的节点(cur)的头节点
            cur = stack.pop();
            result.add(cur.val);
            cur = cur.right;
        }
        return result;
    }
}
```

这里与前和后不同的是，在第二个循环的判断条件里，不会一直加入结果，直到出了这个循环。才会出栈一个节点，加入他的值。然后将下一节点设置为自己的右孩子。



## 题解V3.0 Morris遍历

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        TreeNode predecessor = null;
        TreeNode cur = root;
        while(cur != null){
            //如果没有左孩子，加入答案，访问右孩子
            if (cur.left == null){
                result.add(cur.val);
                cur = cur.right;
            } //有左孩子，找左孩子的最右节点设置为predecessor
            else{
                predecessor = cur.left;
                while(predecessor.right != cur && predecessor.right != null){
                    predecessor = predecessor.right;
                }
                if(predecessor.right == null){
                    predecessor.right = cur; // 能让遍历找到之前的节点，右孩子指向现在的cur
                    cur = cur.left; //继续向左遍历
                }
                else{ //不为空说明，左子树遍历完，已经指到了以前的节点，断开连接
                    result.add(cur.val);
                    predecessor.right = null;
                    cur = cur.right;
                }
            }
        }
        return result;
    }
}
```

与递归和迭代不同，这里不需要维护一个栈来存储之前需要处理的节点。处理完左子树后，直接处理

