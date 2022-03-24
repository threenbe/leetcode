# Two Sum IV - Input is a BST

Given the root of a Binary Search Tree and a target number k, return true if there exist two elements in the BST such that their sum is equal to the given target.

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
    public boolean findTarget(TreeNode root, int k) {
        // BSTs by definition have no duplicates (most of the time...) so we can use a Set to store previous values
        Set<Integer> set = new HashSet<>();
        return find(root, k, set);
    }
    
    private boolean find(TreeNode root, int k, Set<Integer> set) {
        if (root == null)
            return false;
        if (set.contains(k - root.val))
            return true;
        
        set.add(root.val);
        return find(root.left, k, set) || find(root.right, k, set);
    }
}
```
