# Sum Root to Leaf Numbers

ou are given the root of a binary tree containing digits from 0 to 9 only.

Each root-to-leaf path in the tree represents a number.

* For example, the root-to-leaf path 1 -> 2 -> 3 represents the number 123.

Return the total sum of all root-to-leaf numbers. Test cases are generated so that the answer will fit in a 32-bit integer.

A leaf node is a node with no children.

https://leetcode.com/problems/sum-root-to-leaf-numbers/

# My solution:

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
    int sum = 0;
    
    public int sumNumbers(TreeNode root) {
        generatePathNums(root, 0);
        return sum;
    }
    
    private void generatePathNums(TreeNode root, int num) {
        if (root == null)
            return;
        
        if (root.left == null && root.right == null) {
            sum += num*10 + root.val;
        }
        else {
            if (root.left != null)
                generatePathNums(root.left, num*10 + root.val);
            if (root.right != null)
                generatePathNums(root.right, num*10 + root.val);
        }
    }
    // Time complexity: O(n)
    // Space complexity: O(h) for tree height
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
    public int sumNumbers(TreeNode root) {
        int sum = 0;
        List<Integer> pathNums = new ArrayList<>();
        generatePathNums(root, 0, pathNums);
        for (Integer n : pathNums)
            sum += n;
        return sum;
    }
    
    private void generatePathNums(TreeNode root, int num, List<Integer> pathNums) {
        if (root == null)
            return;
        
        if (root.left == null && root.right == null) {
            num = num*10 + root.val;
            pathNums.add(num);
        }
        else {
            if (root.left != null)
                generatePathNums(root.left, num*10 + root.val, pathNums);
            if (root.right != null)
                generatePathNums(root.right, num*10 + root.val, pathNums);
        }
    }
    // Time complexity: O(n)
    // Space complexity: O(h) for tree height plus O(n/2) for potential number of pathNums
}
```
