# Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## My solution: 

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode h = new ListNode(-1);
        ListNode n = h;
        //int ans = 0;
        //int mul = 1;
        int carry = 0;
        while (l1 != null || l2 != null){
            int val1 = 0, val2 = 0;
            if (l1 != null){
                val1 = l1.val;
                l1 = l1.next;
            } 
            if (l2 != null){
                val2 = l2.val;
                l2 = l2.next;
            }
            int tempsum = val1+val2+carry;
            //ans += (tempsum%10)*mul;
            h.next = new ListNode(tempsum%10);
            h = h.next;
            carry = tempsum/10;
            //mul *= 10;
        }
        if (carry != 0){
            //ans += carry*mul;
            h.next = new ListNode(carry);
        }
        //System.out.println(ans);
        return n.next;
    }
}
```