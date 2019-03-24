# Validate Binary Search Tree

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys less than the node's key.
* The right subtree of a node contains only nodes with keys greater than the node's key.
* Both the left and right subtrees must also be binary search trees.

## My solution:

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        //do an inorder traversal and make sure that it is indeed in order
        vector<int> v;
        inorder(root, v);
        for (int i = 1; i < v.size(); i++) {
            if (v[i] <= v[i-1]) return false;
        }
        return true;
    }

private:
    void inorder(TreeNode* root, vector<int> &v) {
        if (root == NULL) return;
        
        inorder(root->left, v);
        v.push_back(root->val);
        inorder(root->right, v);
    }
};
```
## My non-recursive solution:

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        //do an inorder traversal and make sure that it is indeed in order
        //this is a solution that doesn't use recursion
        vector<int> v;
        stack<TreeNode*> s;
        while (root != NULL || !s.empty()) {
            while (root != NULL) {
                //push nodes onto stack until we reach end of left subtree
                s.push(root);
                root = root->left;
            }
            root = s.top();//leftmost child
            s.pop();
            v.push_back(root->val);
            root = root->right;//right subtree
        }
        
        /*
            2
           / \
          1   3
         
         Push 2 onto stack
         Push 1 onto stack
         Pop 1
         v = {1}
         Pop 2 
         v = {1, 2}
         Push 3 onto stack
         Pop 3
         v = {1, 2, 3}
        */
        
        for (int i = 1; i < v.size(); i++) {
            if (v[i] <= v[i-1]) return false;
        }
        return true;
    }
};
```
