# Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

https://leetcode.com/problems/minimum-depth-of-binary-tree/

## My recursive solution (DFS):

```Java
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
    public int minDepth(TreeNode root) {
        if (root == null)
            return 0;
        
        if (root.left == null) // consider only the right subtree if there is no left subtree
            return minDepth(root.right)+1;
        
        if (root.right == null) // consider only left subtree if there is no right subtree
            return minDepth(root.left)+1;
        
        return Math.min(minDepth(root.left), minDepth(root.right))+1;
    }
    
    // Time complexity: O(n), we visit each node at least once
    // Space complexity: O(n)
}
```
