# Two Sum BSTs

Given the roots of two binary search trees, root1 and root2, return true if and only if there is a node in the first tree and a node in the second tree whose values sum up to a given integer target.

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
    public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {
        // Do an inorder traversal of both trees and return a sorted list for both.
        // Create two pointers, set one to point to the start of list1 and set the other
        // to point to the end of list2. If the sum of these two numbers is larger
        // than the target, decrement the second pointer, else increment the first one.
        List<Integer> list1 = new ArrayList<>();
        List<Integer> list2 = new ArrayList<>();
        inorder(root1, list1);
        inorder(root2, list2);
        
        int left = 0;
        int right = list2.size()-1;
        
        while (left < list1.size() && right > 0) {
            int sum = list1.get(left) + list2.get(right);
            if (sum == target)
                return true;
            else if (sum > target)
                right--;
            else
                left++;
        }
        
        return false;
    }
    
    private void inorder(TreeNode root, List<Integer> list) {
        if (root == null)
            return;
        
        inorder(root.left, list);
        list.add(root.val);
        inorder(root.right, list);
    }
    
    // Time complexity: O(N1+N2), where N1+N2 is the sum of number of nodes in each tree
}
```
