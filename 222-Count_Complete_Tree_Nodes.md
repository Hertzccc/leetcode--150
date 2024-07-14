# 222. Count Complete Tree Nodes

## 题目描述

Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `2h` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

 

**Example 1:**

![img](./222-Count_Complete_Tree_Nodes.assets/complete.jpg)

```
Input: root = [1,2,3,4,5,6]
Output: 6
```

**Example 2:**

```
Input: root = []
Output: 0
```

**Example 3:**

```
Input: root = [1]
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5 * 104]`.
- `0 <= Node.val <= 5 * 104`
- The tree is guaranteed to be **complete**.



## 题解V1.0 层序遍历

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
    public int countNodes(TreeNode root) {
        if(root == null){ return 0;}
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int count=0;
        while(!queue.isEmpty()){
            int length = queue.size();
            for(int i=0; i< length; i++){
                TreeNode node = queue.poll();
                count++;
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }
        }
        return count;
    }
}
```



## 题解V2.0 完全二叉树特性

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
    public int countNodes(TreeNode root) {
        if(root == null){ return 0;}
        TreeNode left_part = root.left; //两个指针，一直向左，一直向右
        TreeNode right_part = root.right;
        int leftD = 0, rightD = 0;
        while(left_part!=null){
            left_part = left_part.left;
            leftD++;
        }
        while(right_part!=null){
            right_part = right_part.right;
            rightD++;
        }
        if(leftD == rightD){
            return (2 << leftD) - 1; //初始化leftD为0，这样最后节点(2<<0)-1 = 2;
        }
        //不是满的情况，继续递归(后序)
        int leftNode = countNodes(root.left);  //左
        int rightNode = countNodes(root.right);//右
        return leftNode+rightNode+1;           //中
    }
}
```

上一个针对任意二叉树都可以，但没有用上完全二叉树的特性。

完全二叉树：除最后一层节点没有填满，其余每层h，都有从第一层到该层h节点数为2^h -1个。 h层是可以为0的。

所以现在遍历有两种情况：

- 满二叉树：直接用2^h-1来计算
- 最后一层叶子节点未满：分别递归左右孩子，到某一深度一定会有满二叉树，按照上一种来计算。

如何判断是否为满二叉树？

​	左深度如果等于右深度，就是满二叉树。即一直向左遍历，一直向右遍历，不会出现中间少节点的情况，因为是完全二叉树。

之后遍历是后序遍历，左右中，和之前的方法是一样。只是加了一些判断条件，减小遍历数量。