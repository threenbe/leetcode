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

## Another solution:

I came up with this solution as a means to touch each point of data fewer times than in the above solution. In my first approach, we do a full traversal of both trees to form our lists, and then in the worst case (when there are no two nodes that add up to target) we traverse over every node again in the lists. Meanwhile, in this solution, we only have to traverse the first tree once to form a hashset, and then we have to do a partial or at most one full traversal of the second tree to see if any of its nodes' complements are in the hashset (this lookup is an O(1) operation).

That being said, my first solution was still consistently faster (not that it matters too much, both take <10ms to complete all test cases on leetcode), so I guess HashSets in Java are just that slow.

Also, the first solution is kinda cooler anyway because it actually takes advantage of the fact that the input is a BST by doing the inorder traversal to extract a sorted list. The solution below could be applied to any binary tree. Both of leetcode's solutions, funnily enough, use HashSets like this one does, but they also emphasize the use of inorder traversal when it doesn't really matter. They call out that an inorder traversal results in a sorted list, but they toss the results into a HashSet anyway. Both of their solutions could be applied to any binary tree.

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
        // We can take advantage of the fact that BSTs don't have duplicates and use
        // a HashSet.
        // Do a traversal of the first tree (the exact type of traversal doesn't really)
        // matter here, since hashsets are unordered) and store all of its nodes in a
        // hashset. Then, do a traversal of the second tree, and for each value, check
        // to see if the aforementioned hashset contains its complement.
        
        HashSet<Integer> set = new HashSet<>();
        traverse(root1, set);
        return complementExists(root2, set, target);
    }
    
    private void traverse(TreeNode root, Set<Integer> set) {
        if (root == null)
            return;
        
        traverse(root.left, set);
        set.add(root.val);
        traverse(root.right, set);
    }
    
    private boolean complementExists(TreeNode root, Set<Integer> set, int target) {
        if (root == null)
            return false;
        
        return 
            set.contains(target - root.val) 
            || complementExists(root.left, set, target) || complementExists(root.right, set, target);
    }
    
    // Time complexity: O(N1+N2), where N1+N2 is the sum of number of nodes in each tree
}
```
