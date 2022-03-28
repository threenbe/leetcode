# Linked List Cycle II

Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer.

Do not modify the linked list.

## My solution:

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        // First, figure out if there's a cycle or not
        ListNode intersection;
        if ((intersection = getIntersection(head)) == null) {
            return null;
        }
        //Now, figure out where the cycle starts.
        //Let's say that there are N nodes before the cycle starts,
        //and L nodes from the cycle start to the intersection point
        //at which the slow/fast pointers meet.
        //The slow pointer will have traveled N+X nodes.
        //The fast pointer will have traveled 2*(N+X) nodes. This can
        //also be characterized as N+X+L, where L is the number of nodes
        //in the entire cycle.
        //2*(N+X) = N+X+L -> (N+X)+(N+X) = N+X+L -> N+X=L
        //In other words, the number of nodes before the start of the cycle
        //and the number of nodes from the cycle start to the intersection point
        //is equal to the length of the cycle itself.
        //This means that a pointer starting from the intersection point must travel
        //N nodes to reach the start of the cycle, since N=L-X.
        //So, if you have a pointer that starts from the head of the linked list, and
        //a pointer at the intersection point, and have them both advance by N nodes,
        //then they'll meet at the start of the cycle, which we can return.
        while (head != intersection) {
            head = head.next;
            intersection = intersection.next;
        }
        return head;
    }
    
    private ListNode getIntersection(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (fast == slow) {
                return fast;
            }
        }
        return null;
    }
}
```
