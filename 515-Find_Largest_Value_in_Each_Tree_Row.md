# 515. Find Largest Value in Each Tree Row

## 题目描述

Given the `root` of a binary tree, return *an array of the largest value in each row* of the tree **(0-indexed)**.

 

**Example 1:**

![img](./515-Find_Largest_Value_in_Each_Tree_Row.assets/largest_e1.jpg)

```
Input: root = [1,3,2,5,3,null,9]
Output: [1,3,9]
```

**Example 2:**

```
Input: root = [1,2,3]
Output: [1,3]
```

 

**Constraints:**

- The number of nodes in the tree will be in the range `[0, 104]`.
- `-231 <= Node.val <= 231 - 1`



## 题解V1.0

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
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if(root == null){return result;}
        Queue<TreeNode> queue = new ArrayDeque<>();

        queue.offer(root);
        while(!queue.isEmpty()){
            int level_size = queue.size();
            int largest = Integer.MIN_VALUE;
            for(int i=0; i< level_size; i++){
                TreeNode node = queue.poll();
                largest = (largest > node.val) ? largest : node.val;
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }
            result.add(largest);
        }
        return result;
    }
}
```

这里不是最大的节点，只需要每层的最大数，只需要一个维护一个最大数largest，初始化为最小值Integer.MIN_VALUE；
