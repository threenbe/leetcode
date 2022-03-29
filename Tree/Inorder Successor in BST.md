# Inorder Successor in BST

Given the root of a binary search tree and a node p in it, return the in-order successor of that node in the BST. If the given node has no in-order successor in the tree, return null.

The successor of a node p is the node with the smallest key greater than p.val.

All Nodes will have unique values.

https://leetcode.com/problems/inorder-successor-in-bst/

## My solution:

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        List<TreeNode> nodesInOrder = new ArrayList<>();
        inorder(root, nodesInOrder);
        for (int i = 0; i < nodesInOrder.size()-1; i++) {
            TreeNode node = nodesInOrder.get(i);
            if (node.val == p.val) {
                return nodesInOrder.get(i+1);
            }
        }
        return null;
    }
    
    private void inorder(TreeNode root, List<TreeNode> list) {
        if (root == null)
            return;
        
        inorder(root.left, list);
        list.add(root);
        inorder(root.right, list);
    }
    // Time complexity: O(n), every node is visited
    // Space complexity: O(n), for the space taken up by the call stack during recursion as well as the ordered list of nodes that we construct
}
```
