# Reverse Linked List II

Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.

## My solution

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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode prev = null;
        ListNode curr = head;
        while (left > 1) {//set current node to the "left" position
            prev = curr;
            curr = curr.next;
            left--;
            right--;
        }
        // The node prior to "left" will have to point to the node at "right" after it's reversed
        ListNode prior = prev;
        // The current node ("left") is the head of the sublist we're reversing now,
        // and will thus be the tail of the sublist after we're done reversing. This
        // node will then have to point to the node that comes after "right."
        ListNode subtail = curr;
        
        ListNode next;
        while (right > 0) {
            next = curr.next;
            curr.next = prev;//reverse the link
            prev = curr;//advance
            curr = next;
            right--;
        }
        
        if (prior != null) {
            prior.next = prev;// "prev" is the last node we reversed
        }
        else {//if the head is part of the sublist we reversed, then our new head is "prev"
            head = prev;
        }
        subtail.next = curr;
        
        return head;
    }
}
```
