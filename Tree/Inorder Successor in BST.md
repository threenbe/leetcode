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
        // If p is greater than root, then we know that p's successor
        // cannot possibly be in the root's left subtree. This also goes for
        // when p is the root.
        // If p is less than root, then p is in the root's left subtree. Its
        // successor may be one of the nodes in the left subtree, or it
        // may be the root itself.
        // The problem statement says that the nodes all have unique values so
        // no need to worry about that.
        
        TreeNode successor = null;
        
        while (root != null) {
            if (p.val >= root.val) // skip left subtree
                root = root.right;
            else { // value lies in left subtree, and root might be the successor
                successor = root;
                root = root.left;
                // since we've gone down the left subtree, any subsequent candidates
                // for the successor (i.e. subsequent nodes that are greater than p)
                // are guaranteed to be less than this current root, making them a better
                // candidate for successor
            }
        }
        
        return successor;
        
        // Example:
        /*
                  5
                 / \
                3   6
               / \
              2   4
             /
            1
            p = 4
            
            - Start from root. root.val > p.val, so go left and store root as successor.
            - New root is 3. p.val > root.val, so go right and leave the successor intact.
            - New root is 4. This is our target node, p. Any candidates for its successor
              must be greater than it, so go right and leave the successor intact.
            - Root is now null. The successor must therefore be 5.
            
            Let's say p = 2 instead. Then, when we reach 3, we'd overwrite the successor with
            3 and go right. But 2 has no right subtree, so its successor must be 3.
            Let's say p = 6. At the top root node 5, we'd discard the entire left subtree
            because 6 > 5, and go right. We'd reach 6, but 6 has no right subtree, meaning
            that it is the max node and therefore has no successor. We return null.
        */
    }
    // Time complexity: O(n) in the worst case, when we have a tree that's completely skewed.
    // But for a balanced tree, it will be O(log(n)), since we take advantage of the BST's
    // properties to discard one subtree at every step.
    // Space complexity: O(1)
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
    // Space complexity: O(n), for the space taken up by the call stack during recursion,
    // as well as the ordered list of nodes that we construct
}
```
