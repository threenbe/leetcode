# Reverse Linked List

Given the head of a singly linked list, reverse the list, and return the reversed list.

## My iterative solution:

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
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            //save a reference to the next node because we're about to overwrite it
            ListNode next = curr.next;
            //have the current node point to the previous one
            curr.next = prev;
            //advance forward
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```
