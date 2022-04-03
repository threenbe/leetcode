# Diameter of Binary Tree

Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.

https://leetcode.com/problems/diameter-of-binary-tree/

## My solution:

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
    private int diameter = 0;
    
    public int diameterOfBinaryTree(TreeNode root) {
        // Another way to frame this problem is to find
        // the longest path between two leaf nodes.
        // So, at every node, we'll want to get the longest
        // depth for each of its subtrees and add them together,
        // and then check to see if that surpasses our current
        // maximum path length.
        longestPath(root);
        return diameter;
    }
    
    private int longestPath(TreeNode root) {
        if (root == null)
            return 0;
        
        int leftSubtreeDepth = longestPath(root.left);
        int rightSubtreeDepth = longestPath(root.right);
        
        // Calculate the length of the longest path that runs through
        // the current root node.
        diameter = Math.max(diameter, leftSubtreeDepth + rightSubtreeDepth);
        
        // Return the depth to the parent node that called this method so
        // that it can also calculate the longest path that runs through it.
        return Math.max(leftSubtreeDepth, rightSubtreeDepth) + 1;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
