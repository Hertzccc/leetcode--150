# 199. Binary Tree Right Side View

## 题目描述

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return *the values of the nodes you can see ordered from top to bottom*.

 

**Example 1:**

<img src="./199-Binary_Tree_Right_Side_View.assets/tree.jpg" alt="img" />

```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
```

**Example 2:**

```
Input: root = [1,null,3]
Output: [1,3]
```

**Example 3:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

## 题解V1.0 Queue遍历

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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if(root == null){ return result;}
        Queue<TreeNode> queue = new ArrayDeque<>();

        queue.offer(root);
        while(!queue.isEmpty()){
            int level_size = queue.size();
            TreeNode node = null;
            for(int i=0; i< level_size; i++){
                node = queue.poll();//获取当前queue顶值
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }
            result.add(node.val);
        }
        return result;
    }
}
```

这个与102的区别也不大，不用多维护一个存储每层节点的ArrayList，在遍历结束后，直接添加进result即可。