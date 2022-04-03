# Path Sum III

Given the root of a binary tree and an integer targetSum, return the number of paths where the sum of the values along the path equals targetSum.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

https://leetcode.com/problems/path-sum-iii/

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
    private int count = 0;
    // key, val -> prefix sum, the number of times this sum has occurred
    private HashMap<Integer, Integer> map = new HashMap<>();
    
    // DFS
    private void traversePaths(TreeNode root, int currentSum, int targetSum) {
        // We can solve this with the prefix sum pattern.
        // If we're traveling down a tree, and we encounter a prefix sum
        // equal to our targetSum, then we increment our count. Also, if 
        // our current prefix sum minus the targetSum has been encountered
        // before, then that means there is also a subpath on our current
        // path that sums to targetSum.
        // One thing to keep in mind is that each parent-leaf path needs to
        // be evaluated separately.
        
        if (root == null)
            return;
        
        currentSum += root.val;
        
        if (currentSum == targetSum)
            count++;
        
        count += map.getOrDefault(currentSum - targetSum, 0);
        
        map.put(currentSum, map.getOrDefault(currentSum, 0) + 1);
        
        if (root.left != null)
            traversePaths(root.left, currentSum, targetSum);
        if (root.right != null)
            traversePaths(root.right, currentSum, targetSum);
        
        map.put(currentSum, map.get(currentSum) - 1);
    }
    
    public int pathSum(TreeNode root, int targetSum) {
        traversePaths(root, 0, targetSum);
        return count;
    }
}
```

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
