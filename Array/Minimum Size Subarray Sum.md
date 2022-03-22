# Minimum Size Subarray Sum

Given an array of positive integers nums and a positive integer target, return the minimal length of a contiguous subarray [numsl, numsl+1, ..., numsr-1, numsr] of which the sum is greater than or equal to target. If there is no such subarray, return 0 instead.

https://leetcode.com/problems/minimum-size-subarray-sum/

## My solution:

```Java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int right = 0;
        int minLength = Integer.MAX_VALUE;
        int currentSum = 0;

        while (right < nums.length) {
            currentSum += nums[right++];
            while (currentSum >= target) {
                int currentLength = right-left;
                minLength = Math.min(minLength, currentLength);
                currentSum -= nums[left++];
            }
        }
        
        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
}
```
