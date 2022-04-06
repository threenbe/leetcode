# Single Number II

Given an integer array nums where every element appears three times except for one, which appears exactly once. Find the single element and return it.

You must implement a solution with a linear runtime complexity and use only constant extra space.

https://leetcode.com/problems/single-number-ii/

## My solution:

```Java
class Solution {
    // XOR operation lets us identify when an element appears once or thrice,
    // but we need to differentiate between the two.
    // In the easier version of this problem where the duplicates only appear
    // twice, we just XOR every number together. The ones that appear twice
    // disappear and the one that appears once remains.
    // We need a way to keep track of when an element gets seen a second time
    // and then somehow use that to make it "disappear" when it gets seen a third
    // time. Basically, since we used one variable to store two states last time, we
    // should be able to use two variables to store three states this time.
    // For the case in which duplicates appear twice, let's consider an "input" and a
    // singular variable "x" to track state. x represents the state before we see input,
    // and x' represents the state after we see input.
    /* x | input | x'
       0     0     0
       0     1     1
       1     0     1
       1     1     0
    */ 
    // The above truth table is just the truth table for an XOR. We start with 0. We see a
    // number once (say it's just a bit for simplicity), 0^1 = 1. We see it again, 1^1 = 0.
    // It's gone. To illustrate with a bigger number, 0000^0010 = 0010. 0010^0010 = 0000.
    // Now let's think about using two variables to store three states.
    // 2 = 00 10
    // seen_once = 00 00, seen_twice = 00 00
    // Encounter 2 once -> seen_once = 00 10, seen_twice = 00 00
    // Encounter 2 twice -> seen_once = 00 00, seen_twice = 00 10
    // Encounter 2 thrice -> seen_once = 00 00, seen_twice = 00 00 
    // now, how do we do this^
    // For simplicity's sake let's just examine the bit on its own.
    // Let's define the bit as an "input" 1. s1 and s2 are seen_once and seen_twice after
    // the input is encountered once. s1' and s2' are seen_once and seen_twice after the input
    // is encountered twice. Let's fill out a truth table based on the above.
    /* s1 | s2 | input | s1' | s2'
        0    0     0      0     0
        1    0     0      1     0
        0    1     0      0     1
        0    0     1      1     0
        1    0     1      0     1
        0    1     1      0     0
    */
    // We can see that s1' = input^s1 on every row except for when s2 and input are both 1
    // s1' = ~s2 & (s1^input) -- this works for the last row, and it works for the rest too.
    // We can see that s2' = input^s2 on every row except for when s1 is 0 and input is 1.
    // One thing to keep in mind though is that we're going to calculate s1 before
    // s2, so we should also consider what s1' is. s2' = input^s2 on every row except when
    // s1' and input are both 1. So...
    // s2' = ~s1' & (s2^input)
    // In code, if we just do something like...
    // seen_once = ~seen_twice & (seen_once^input)
    // seen_twice = ~seen_once & (seen_twice^input) --- note that we've already modified
    //                                                  seen_once in the previous line, so
    //                                                  this is really s1', not s1
    // ...then we should be able to erase numbers that appear thrice while preserving the one
    // that appears once.
    public int singleNumber(int[] nums) {
        int seenOnce = 0;
        int seenTwice = 0;
        for (int n : nums) {
            seenOnce = ~seenTwice & (seenOnce^n);
            seenTwice = ~seenOnce & (seenTwice^n);
        }
        return seenOnce;
    }
    // Time complexity: O(n)
    // Space complexity: O(1)
}
```
