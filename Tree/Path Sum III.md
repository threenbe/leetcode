# Path Sum III

Given the root of a binary tree and an integer targetSum, return the number of paths where the sum of the values along the path equals targetSum.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

https://leetcode.com/problems/path-sum-iii/

## My initial solution:

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
    public int pathSum(TreeNode root, int targetSum) {
        // A path may or may not include the root node.
        // This means that when we're at the root, we consider
        // two options: one in which we include the root in our
        // calculation, and one in which we don't include it and
        // instead start a path from one of our child nodes.
        if (root == null)
            return 0;
        
        return pathSumFromNode(root, targetSum) + 
            pathSum(root.left, targetSum) + pathSum(root.right, targetSum);
    }
    
    private int pathSumFromNode(TreeNode root, int targetSum) {
        if (root == null)
            return 0;
        
        return (root.val == targetSum ? 1 : 0)
            + pathSumFromNode(root.left, targetSum - root.val)
            + pathSumFromNode(root.right, targetSum - root.val);
    }
    
    // Time complexity: O(n^2), we check each path starting from the root node, and then
    // each path from its left child, and then each path from its left child, etc, and then
    // repeat with the right tree, etc.
    // Space complexity: O(n) due to recursion.
}
```
