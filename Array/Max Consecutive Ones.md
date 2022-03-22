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
