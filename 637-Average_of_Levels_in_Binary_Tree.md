# 637. Average of Levels in Binary Tree

## 题目描述

Given the `root` of a binary tree, return *the average value of the nodes on each level in the form of an array*. Answers within `10-5` of the actual answer will be accepted.

 

**Example 1:**

<img src="./637-Average_of_Levels_in_Binary_Tree.assets/avg1-tree.jpg" alt="img" />

```
Input: root = [3,9,20,null,null,15,7]
Output: [3.00000,14.50000,11.00000]
Explanation: The average value of nodes on level 0 is 3, on level 1 is 14.5, and on level 2 is 11.
Hence return [3, 14.5, 11].
```

**Example 2:**

<img src="./637-Average_of_Levels_in_Binary_Tree.assets/avg2-tree.jpg" alt="img" />

```
Input: root = [3,9,20,15,7]
Output: [3.00000,14.50000,11.00000]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-231 <= Node.val <= 231 - 1`

## 题解V1.0 Queue

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
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> result = new ArrayList<>();
        if(root == null)  return result;
        Queue<TreeNode> queue = new ArrayDeque<>();

        queue.offer(root);
        while(!queue.isEmpty()){
            int level_size = queue.size();//获取层数
            double level_sum = 0.0;
            for(int i=0; i< level_size; i++){
                TreeNode node = queue.poll();//获取queue头
                level_sum += node.val;
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }
            result.add(level_sum/level_size);
        } 
        return result;
    }
}
```

还是和102类似，只需要维护一个每层的level_sum用来储存每层的加和。