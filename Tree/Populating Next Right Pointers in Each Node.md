# Populating Next Right Pointers in Each Node

You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```C
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

https://leetcode.com/problems/populating-next-right-pointers-in-each-node/

## My solution:

```Java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        if (root == null)
            return null;
        
        Queue<Node> q = new ArrayDeque<>();
        q.add(root);
        while(!q.isEmpty()) {
            int size = q.size();
            List<Node> currentLevel = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                Node node = q.poll();
                if (node.left != null)
                    q.add(node.left);
                if (node.right != null)
                    q.add(node.right);
                currentLevel.add(node);
            }
            int i;
            for (i = 0; i < size-1; i++) {
                Node n1 = currentLevel.get(i);
                Node n2 = currentLevel.get(i+1);
                n1.next = n2;
            }
            currentLevel.get(i).next = null;
        }
        
        return root;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
