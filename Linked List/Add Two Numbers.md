# Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## Python solution:

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        # Example: l1 = [2,4,3] + l2 = [5,6,4] == result = [7,0,8]
        # 342 + 465 = 807
        # First digit: 2+5 = 7
        # Second digit: 4+6 = 10 -> take 0, carry the 1; 07
        # Third digit: 3+4 = 7; add the carry, 7+1 = 8; 807
        head = ListNode(0)
        tail = head # append new values using the tail
        carry = 0

        while l1 is not None or l2 is not None or carry != 0:
            digit1 = l1.val if l1 is not None else 0
            digit2 = l2.val if l2 is not None else 0
            tempSum = digit1 + digit2 + carry

            tail.next = ListNode(tempSum % 10)
            tail = tail.next # move tail forward upon adding a new value
            carry = tempSum // 10 # must use floor division, else you get decimal results

            l1 = l1.next if l1 is not None else None
            l2 = l2.next if l2 is not None else None

        return head.next
```

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
