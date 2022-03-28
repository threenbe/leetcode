# Happy Number

Write an algorithm to determine if a number n is happy.

A happy number is a number defined by the following process:

Starting with any positive integer, replace the number by the sum of the squares of its digits.
Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
Those numbers for which this process ends in 1 are happy.
Return true if n is a happy number, and false if not.

https://leetcode.com/problems/happy-number/

## My solution:

```Java
class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = getNext(n);
        //if n is not a happy number then it'll eventually cycle between several
        //numbers over and over, so we can detect this cycle with a slow and fast
        //pointer. Otherwise fast will eventually equal 1.
        while (fast != slow && fast != 1) {
            slow = getNext(slow);
            fast = getNext(getNext(fast));
        }
        return fast == 1;
    }
    
    private int getNext(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            n /= 10;
            sum += digit*digit;
        }
        return sum;
    }
}
```
