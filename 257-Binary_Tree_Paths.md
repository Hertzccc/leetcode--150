# 257. Binary Tree Paths

## 题目描述

Given the `root` of a binary tree, return *all root-to-leaf paths in **any order***.

A **leaf** is a node with no children.

 

**Example 1:**

![img](./257-Binary_Tree_Paths.assets/paths-tree.jpg)

```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
```

**Example 2:**

```
Input: root = [1]
Output: ["1"]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
- `-100 <= Node.val <= 100`

## 题解V1.0 递归回溯

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
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        if(root == null){ return result;}
        List<Integer> path = new ArrayList<>();
        traversal(root, result, path);
        return result;
    }
    public void traversal(TreeNode node, List<String> result, List<Integer> path){
        //前序遍历，中左右、
        path.add(node.val);
        //判断是否为叶子节点
        if(node.left == null && node.right == null){
            StringBuilder sb = new StringBuilder();
            sb.append(path.get(0));
            for(int i=1; i<path.size(); i++){
                sb.append("->").append(path.get(i));
            }
            result.add(sb.toString());
            return;
        }
        if(node.left != null){
            traversal(node.left, result, path);
            path.remove(path.size()-1);
        }
        if(node.right != null){
            traversal(node.right, result, path);
            path.remove(path.size()-1);
        }
    }
}
```

维护两个List，一个是result储存结果，一个是path路径储存中间结果。

递归采用前序遍历，中左右。

- 先确认返回值为void，不需要返回值。递归参数，每次传入的是节点，还有两个List，result和path。

- 递归出口：递归到叶子节点时，这个时候还需要一系列处理。
- 回溯的过程也就在递归中，path.remove(last),直接删除刚刚遍历的节点。



## 题解V2.0 用StringBuilder传参

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
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        if(root == null){ return result;}
        StringBuilder path = new StringBuilder();
        traversal(root, result, path);
        return result;
    }
    public void traversal(TreeNode node, List<String> result, StringBuilder path){
        //前序遍历，中左右、
        //判断是否为叶子节点
        int length = path.length();
        if(node.left == null && node.right == null){
            result.add(path.append(node.val).toString());
            }
        StringBuilder sb = new StringBuilder(path.append(node.val).append("->"));
        if(node.left != null){
            traversal(node.left, result, sb);
        }
        if(node.right != null){
            traversal(node.right, result, sb);
        }
        path.setLength(length);
    }
}
```

这里传入StringBuilder参数，用`append`拼接`sb`是比`+`快很多的。

但需要手动回溯 `path.setLength(length);`，因为SB是可变字符串。

## 题解V2.1 用String传参

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
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        if(root == null){ return result;}
        traversal(root, result, "");
        return result;
    }
    public void traversal(TreeNode node, List<String> result, String path){
        //前序遍历，中左右、
        //判断是否为叶子节点
        if(node.left == null && node.right == null){
            result.add(new StringBuilder(path).append(node.val).toString());
            }
        String s = new StringBuilder(path).append(node.val).append("->").toString();
        if(node.left != null){
            traversal(node.left, result, s);
        }
        if(node.right != null){
            traversal(node.right, result, s);
        }
    }
}
```

这里选择传入String，不用手动回溯，String是不可变，传入后都是一个新的对象。不过这个不如上一个题解V2.0，因为上一个只需要维护一个SB的对象。