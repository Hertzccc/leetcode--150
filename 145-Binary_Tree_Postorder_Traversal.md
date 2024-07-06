# 145. Binary Tree Postorder Traversal



## 题目描述

Given the `root` of a binary tree, return *the postorder traversal of its nodes' values*.

 

**Example 1:**

![img](./145-Binary_Tree_Postorder_Traversal.assets/pre1.jpg)

```
Input: root = [1,null,2,3]
Output: [3,2,1]
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

- The number of the nodes in the tree is in the range `[0, 100]`.
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
    public static List<Integer> result;
    public List<Integer> postorderTraversal(TreeNode root) {
        result = new ArrayList<>();
        postorder(root);
        return result;
    }
    public static void postorder(TreeNode node){
        //recursive base case
        if(node == null) {return;}
        postorder(node.left);
        postorder(node.right);
        result.add(node.val);
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
    public List<Integer> postorderTraversal(TreeNode root) {
        //后序遍历 左 右 头,preorder遍历时头左右，
        //可以用preorder调整一下头右左，然后最后翻转数组
        Deque<TreeNode> stack = new ArrayDeque<>();
        List<Integer> result = new ArrayList<>();
        if (root == null) { return result;}

        TreeNode cur = root;
        while(cur!=null || !stack.isEmpty()){
            //右下
            while(cur!=null){
                result.add(cur.val);
                stack.push(cur);
                cur = cur.right;
            }
            cur = stack.pop(); //弹出一个来遍历左边
            cur = cur.left;
        }
        Collections.reverse(result);
        return result;
    }
}
```

