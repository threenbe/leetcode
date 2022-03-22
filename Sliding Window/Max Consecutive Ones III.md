# Max Consecutive Ones III

Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

https://leetcode.com/problems/max-consecutive-ones-iii/

## My solution:

```Java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int maxLength = 0;
        int numZeroes = 0;
        int left = 0, right = 0;
        // Our subarray must maintain the property that numZeroes <= k.
        // We can expand the subarray until doing so would break the above
        // property (making numZeroes exceed k), after which we shrink our window
        // until we satisfy the above property again.
        while (right < nums.length) {
            if (nums[right++] == 0) {
                numZeroes++;
            }
            while (numZeroes > k) {
                if (nums[left++] == 0) {
                    numZeroes--;
                }
            }
            maxLength = Math.max(maxLength, right-left);
        }
        
        return maxLength;
    }
}
```
