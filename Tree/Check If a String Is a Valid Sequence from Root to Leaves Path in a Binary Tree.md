# Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree

Given a binary tree where each path going from the root to any leaf form a valid sequence, check if a given string is a valid sequence in such binary tree. 

We get the given string from the concatenation of an array of integers arr and the concatenation of all values of the nodes along a path results in a sequence in the given binary tree.

https://leetcode.com/problems/check-if-a-string-is-a-valid-sequence-from-root-to-leaves-path-in-a-binary-tree/

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
    public boolean isValidSequence(TreeNode root, int[] arr) {
        return isValidSequence(root, arr, 0);
    }
    
    private boolean isValidSequence(TreeNode root, int[] arr, int idx) {
        if (root == null || idx >= arr.length)
            return false;
        
        if (root.val == arr[idx]) {
            // Valid sequences must be root-to-leaf paths.
            if (root.left == null && root.right == null && idx == arr.length-1)
                return true;
            else
                return isValidSequence(root.left, arr, idx+1) || isValidSequence(root.right, arr, idx+1);
        }
        else {
            return false;
        }
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
