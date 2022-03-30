# Path Sum II

Given the root of a binary tree and an integer targetSum, return all root-to-leaf paths where the sum of the node values in the path equals targetSum. Each path should be returned as a list of the node values, not node references.

A root-to-leaf path is a path starting from the root and ending at any leaf node. A leaf is a node with no children.

https://leetcode.com/problems/path-sum-ii/

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
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> allPathsToSum = new ArrayList<>();
        List<Integer> pathToSum = new ArrayList<>();
        getPathToSums(root, targetSum, pathToSum, allPathsToSum);
        return allPathsToSum;
    }
    
    private void getPathToSums(TreeNode root, int targetSum, 
                               List<Integer> pathToSum, 
                               List<List<Integer>> allPathsToSum) {
        
        if (root == null)
            return;
        
        pathToSum.add(root.val);
        
        if (root.left == null && root.right == null && root.val == targetSum) {
            // Add the current path to the output list
            allPathsToSum.add(new ArrayList<Integer>(pathToSum));
        }
        else {
            if (root.left != null)
                getPathToSums(root.left, targetSum - root.val, pathToSum, allPathsToSum);
            if (root.right != null)
                getPathToSums(root.right, targetSum - root.val, pathToSum, allPathsToSum);
        }
        
        // After processing the current node's subtrees, we pop it from the list
        // as we have finished evaluating the path that leads up to this point.
        // We can presume that it's the last one in the list since all of its child
        // nodes must have been popped in the above function calls. This will then
        // return control to the function call responsible for processing this node's parent.
        pathToSum.remove(pathToSum.size()-1);
    }
    
    // Time complexity: O(n^2), we traverse the tree once, but we also copy the list of nodes
    // in our path to the output list of lists every time we find a sum, which is a O(n)
    // operation.
    // Space complexity: O(n) or O(n^2) depending on whether you count the output list of
    // lists or not.
}
```
