# Single Number

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

## My solution:

```Java
class Solution {
    public int singleNumber(int[] nums) {
        int num = nums[0];
        for (int i = 1; i < nums.length; i++)
            num ^= nums[i];
        return num;
    }
    // Time complexity: O(n)
    // Space complexity: O(1)
}
```
