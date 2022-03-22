# Max Consecutive Ones

Given a binary array nums, return the maximum number of consecutive 1's in the array.

## My solution:

```Java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int currentCount = 0;
        int maxCount = 0;
        for (int num : nums) {
            if (num == 1)
                currentCount++;
            else
                currentCount = 0;
            
            maxCount = Math.max(currentCount, maxCount);
        }
        
        return maxCount;
    }
}
```

## Extra (in every sense of the word) solution:

```Java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int maxCount = 0;
        int left = 0, right = 0;
        while (right < nums.length) {
            int num = nums[right++];
            if (num != 1) {
                left = right;
            }
            
            maxCount = Math.max(maxCount, right-left);
        }
        
        return maxCount;
    }
}
```
