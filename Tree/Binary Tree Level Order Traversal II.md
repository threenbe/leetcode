# Binary Tree Level Order Traversal II

Given the root of a binary tree, return the bottom-up level order traversal of its nodes' values. (i.e., from left to right, level by level from leaf to root).

https://leetcode.com/problems/binary-tree-level-order-traversal-ii/

## My solution:

I initially had the solution for the top-down level order traversal in mind, so my brain immediately went "use a stack to do it backwards" for my first solution.
But all you really need is a data structure that can add each successive level to the front, so that by the end, the first entry in the list is the last level, and so on. A doubly-linked list allows you to add elements at the head in O(1) time.

Incidentally, with Java's LinkedList, literally all you have to change here to get the solution for the top-down BFS problem is change the "addFirst" to "addLast."

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        LinkedList<List<Integer>> result = new LinkedList<>();
        
        if (root == null) {
            return result;
        }
        
        // BFS
        Queue<TreeNode> q = new ArrayDeque<>();
        q.add(root);
        while (!q.isEmpty()) {
            int size = q.size();
            List<Integer> currentLevel = new ArrayList<>();
            
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                if (node.left != null) {
                    q.add(node.left);
                }
                if (node.right != null) {
                    q.add(node.right);
                }
                currentLevel.add(node.val);
            }
            
            result.addFirst(currentLevel);
        }
        
        return result;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        Stack<List<Integer>> stack = new Stack<>();
        
        if (root == null) {
            return result;
        }
        
        // BFS
        Queue<TreeNode> q = new ArrayDeque<>();
        q.add(root);
        while (!q.isEmpty()) {
            int size = q.size();
            List<Integer> currentLevel = new ArrayList<>();
            
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                if (node.left != null) {
                    q.add(node.left);
                }
                if (node.right != null) {
                    q.add(node.right);
                }
                currentLevel.add(node.val);
            }
            
            stack.add(currentLevel);
        }
        
        while (!stack.isEmpty()) {
            result.add(stack.pop());
        }
        
        return result;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
