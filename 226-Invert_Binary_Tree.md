# 226. Invert Binary Tree

## 题目描述

Given the `root` of a binary tree, invert the tree, and return *its root*.

 

**Example 1:**

![img](./226-Invert_Binary_Tree.assets/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

**Example 2:**

![img](./226-Invert_Binary_Tree.assets/invert2-tree.jpg)

```
Input: root = [2,1,3]
Output: [2,3,1]
```

**Example 3:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`



## 题解V1.0 queue层序遍历

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
    public TreeNode invertTree(TreeNode root) {
        if(root == null) { return root;}
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int length = queue.size();
            for(int i=0; i<length; i++){
                TreeNode node = queue.poll();
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
                TreeNode temp = node.left;
                node.left = node.right;
                node.right = temp;
            }
        }
        return root;
    }
}
```

直接每层翻转左右孩子即可。



## 题解V2.0 递归

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
    public TreeNode invertTree(TreeNode root) {
        if(root == null) { return root;}
        //pre-order dfs 中左右
        TreeNode temp = root.left;  //中
        root.left = root.right;
        root.right = temp;
        invertTree(root.left);      //左
        invertTree(root.right);     //右
        return root;
    }
}
```

这里采用前序遍历，不能用中序。如果是in-order中序（左右中），



## 题解V3.0 Stack 迭代

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
    public TreeNode invertTree(TreeNode root) {
        if(root == null) { return root;}
        Deque<TreeNode> stack = new ArrayDeque<>(); //用ArrayDeque来代替Stack
        TreeNode cur = root;
        while(cur!=null || !stack.isEmpty()){
            while(cur!=null){
                TreeNode temp = cur.left; //中
                cur.left = cur.right;
                cur.right = temp;
                stack.push(cur);
                cur = cur.left; //左
            }
            cur = stack.pop();
            cur = cur.right; //右
        }
        return root;
    }
}
```

还是采用中、左、右的顺序

另一种写法：

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
    public TreeNode invertTree(TreeNode root) {
        if(root == null) { return root;}
        Deque<TreeNode> stack = new ArrayDeque<>(); //用ArrayDeque来代替Stack
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode cur = stack.pop();//中
            TreeNode temp = cur.left; //swap
            cur.left = cur.right;
            cur.right = temp;

            if(cur.right != null){
                stack.push(cur.right);
            }
            if(cur.left != null){
                stack.push(cur.left);
            }
        }
        return root;
    }
}
```

