# Rotate List

Given the head of a linked list, rotate the list to the right by k places.

https://leetcode.com/problems/rotate-list/

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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) {
            return head;
        }
        // Relative to the old head, if there are n nodes total, then the new head is
        // n-k nodes ahead of the old head. For example, if n=5 and k=1, then the new head
        // is just the old tail, which is 4 nodes ahead of the old head.
        int nodeCount = 0;
        ListNode tail = null;
        ListNode curr = head;
        while (curr != null) {
            tail = curr;
            curr = curr.next;
            nodeCount++;
        }
        k = k % nodeCount;
        if (k == 0) {
            return head;
        }
        
        int newHeadPosition = nodeCount - k;
        ListNode newTail = null;
        ListNode newHead = head;
        while (newHeadPosition > 0) {
            newTail = newHead;
            newHead = newHead.next;
            newHeadPosition--;
        }
        
        //The old tail will point to the old head
        tail.next = head;
        //The new tail is currently pointing to the new head, so we set it to point to nothing
        newTail.next = null;
        //return the new head
        return newHead;
    }
}
```
