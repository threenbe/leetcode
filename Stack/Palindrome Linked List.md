# Palindrome Linked List

Given the head of a singly linked list, return true if it is a palindrome.

https://leetcode.com/problems/palindrome-linked-list/

## Reverse the second half of the list:

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
        // Get the middle of the linked list
        // Reverse the second half of the linked list
        // Check the nodes of the first and second halves one-by-one
        ListNode endOfFirstHalf = getMiddle(head);
        ListNode startOfSecondHalf = reverseList(endOfFirstHalf);
        
        ListNode p1 = head;
        ListNode p2 = startOfSecondHalf;
        while (p2 != null) {
            if (p1.val != p2.val) {
                return false;
            }
            p1 = p1.next;
            p2 = p2.next;
        }
        
        return true;
    }
    
    private ListNode getMiddle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
    
    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            // save reference to the next node
            ListNode next = curr.next;
            // have the current node point to the previous one
            curr.next = prev;
            // advance forward in the list
            prev = curr;
            curr = next;
        }
        return prev;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(1)
}
```

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
