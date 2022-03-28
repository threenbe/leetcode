# Palindrome Linked List

Given the head of a singly linked list, return true if it is a palindrome.

https://leetcode.com/problems/palindrome-linked-list/

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
    public boolean isPalindrome(ListNode head) {
        // Push all values of the linked list onto a stack
        // Pop them out
        // If the order of values is the same, it's a palindrome
        Stack<Integer> stack = new Stack<>();
        ListNode node = head;
        while (node != null) {
            stack.push(node.val);
            node = node.next;
        }
        
        node = head;
        while (node != null) {
            if (node.val != stack.pop()) {
                return false;
            }
            node = node.next;
        }
        
        return true;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
