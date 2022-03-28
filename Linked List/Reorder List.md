# Reorder List

You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln

Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

https://leetcode.com/problems/reorder-list/

## My solution:

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public void reorderList(ListNode head) {
        // So one way you can do this with O(1) space complexity is by:
        // - Finding the middle node
        // - Reversing the second half of the linked list
        // - Merging the first and second halves of the linked list by placing the first
        //   node of the second list in front of the first node of the first list, and so on
        // I don't have the patience for that right now so I'll just use more space
        
        if (head == null || head.next == null)
            return;
        
        Stack<ListNode> stack = new Stack<>();
        ListNode node = head;
        while (node != null) {
            stack.push(node);
            node = node.next;
        }
        
        // Example:
        // 1 -> 2 -> 3 -> 4 -> 5, Stack: [5,4,3,2,1]
        // pop 5, 1 -> 5 -> 2 -> 3 -> 4 -> 5, Stack: [4,3,2,1], "node" is now pointing to 2
        // pop 4, 1 -> 5 -> 2 -> 4 -> 3 -> 4 -> 5, Stack: [3,2,1], "node" is now pointing to 3
        // Note that we have 4 -> 3 -> 4, ergo node.next is equal to the last "end" node we found
        // We know that we can stop here, and set node.next's value to null
        // 1 -> 5 -> 2 -> 4 -> 3 -> null
        
        // Let's run through an example with an even number of nodes
        // 1 -> 2 -> 3 -> 4, Stack: [4,3,2,1]
        // pop 4, 1 -> 4 -> 2 -> 3 -> 4, Stack: [3,2,1], "node" is now pointing to 2
        // pop 3, 1 -> 4 -> 2 -> 3 -> 3 -> 4, Stack: [2,1], "node" is now pointing to the second 3
        // Note that 3 -> 3, this means that the first 3 (right behind "node") is the end of our list:
        // 1 -> 4 -> 2 -> 3 -> null
        node = head;
        while (!stack.isEmpty()) {
            // last node goes in front of the first node, and so on
            ListNode next = node.next;
            ListNode end = stack.pop();
            
            node.next = end;
            end.next = next;
            
            // check to see if we've reached the end
            node = next; // the next node in the first half that could precede a node from the second half
            if (node.next == end) {
                node.next = null;
                break;
            } 
            else if (end == node) {
                end.next = null;
                break;
            }
        }
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
