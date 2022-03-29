# Binary Tree Zigzag Level Order Traversal

Given the root of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate between).

https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        /* Example:
                  1
                 / \
                2   3
               /     \
              4       5
           Zigzag traversal: [[1],[3,2],[4,5]]
        */
        
        List<List<Integer>> result = new ArrayList<>();
        
        if (root == null) {
            return result;
        }
        
        Queue<TreeNode> q = new ArrayDeque<>();
        q.add(root);
        boolean leftFirst = true;
        
        while (!q.isEmpty()) {
            int size = q.size();
            LinkedList<Integer> currentLevel = new LinkedList<>();
            
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                
                if (node.left != null) {
                    q.add(node.left);
                }
                if (node.right != null) {
                    q.add(node.right);
                }
                
                if (leftFirst) {
                    currentLevel.addLast(node.val);
                }
                else {
                    currentLevel.addFirst(node.val);
                }
            }
            
            result.add(currentLevel);
            leftFirst = !leftFirst;
            
        }
        
        return result;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
