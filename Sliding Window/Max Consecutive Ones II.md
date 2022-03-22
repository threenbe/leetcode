# Max Consecutive Ones II

Given a binary array nums, return the maximum number of consecutive 1's in the array if you can flip at most one 0.

https://leetcode.com/problems/max-consecutive-ones-ii/

## My solution:

```Java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int maxLength = 0;
        int numZeroes = 0;
        int left = 0, right = 0;
        // Our subarray must maintain the property that numZeroes <= 1.
        // We can expand the subarray until doing so would break the above
        // property (making numZeroes exceed 1), after which we shift our entire 
        // window to the right until our property is satisfied again.
        while (right < nums.length) {
            if (nums[right++] == 0) {
                numZeroes++;
            }
            if (numZeroes > 1 && nums[left++] == 0) {
                numZeroes--;
            }
            maxLength = Math.max(maxLength, right-left);
        }
        
        return maxLength;
    }
}
```

## My slightly different solution:

```Java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int maxLength = 0;
        int numZeroes = 0;
        int left = 0, right = 0;
        // Our subarray must maintain the property that numZeroes <= 1.
        // We can expand the subarray until doing so would break the above
        // property (making numZeroes exceed 1), after which we shrink our window
        // until we satisfy the above property again.
        while (right < nums.length) {
            if (nums[right++] == 0) {
                numZeroes++;
            }
            while (numZeroes > 1) {
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
