# Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

https://leetcode.com/problems/minimum-depth-of-binary-tree/

## My iterative solution (BFS):

Visits fewer nodes than the DFS solution on average (which always visits all of them) and runs much faster on average overall.

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
        
        Queue<TreeNode> q = new ArrayDeque<>();
        q.add(root);
        int level = 1;
        boolean foundLeaf = false;
        // Do a BFS and simply return the level we're at when we encounter our first leaf node
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                
                //check if this is a leaf node
                if (node.left == null && node.right == null) {
                    foundLeaf = true;
                    break;
                }
                
                if (node.left != null)
                    q.add(node.left);
                if (node.right != null)
                    q.add(node.right);
            }
            
            if (foundLeaf) break;
            level++;
        }
        
        return level;
    }
    
    // Time complexity: O(n), if the tree is completely unbalanced then we have to visit every
    // node, and if it's balanced then we have to visit n/2 nodes (we find the min depth at
    // the bottom level's first leaf node), which still simplifies out to O(n)
    // Space complexity: O(n)
}
```

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
