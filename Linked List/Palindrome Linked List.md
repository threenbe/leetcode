# Palindrome Linked List

Given the head of a singly linked list, return true if it is a palindrome.

https://leetcode.com/problems/palindrome-linked-list/

## ArrayList/Two Pointer solution:

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
        // We can use two pointers starting at the beginning and end of the list
        // to compare elements, although that doesn't work with a singly linked list.
        // We have to move the elements into an array.
        List<Integer> list = new ArrayList<>();
        
        ListNode node = head;
        while (node != null) {
            list.add(node.val);
            node = node.next;
        }
        
        int left = 0;
        int right = list.size() - 1;
        while (left < right) {
            if (!list.get(left++).equals(list.get(right--))) {
                return false;
            }
        }
        
        return true;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```

## Stack solution:

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
