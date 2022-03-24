# 3Sum Closest

Given an integer array nums of length n and an integer target, find three integers in nums such that the sum is closest to target.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.

https://leetcode.com/problems/3sum-closest/

## My solution:

```Java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int minDiffFromTarget = Integer.MAX_VALUE;
        int sumClosestToTarget = 0;
        Arrays.sort(nums);
        for (int i = 0; i < nums.length-2; i++) {
            int left = i+1;
            int right = nums.length-1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == target) {
                    return sum;
                }
                if (minDiffFromTarget > Math.abs(sum - target)) {
                    minDiffFromTarget = Math.abs(sum - target);
                    sumClosestToTarget = sum;
                }
                
                if (sum < target) {
                    left++;
                }
                else {
                    right--;
                }
            }
        }
        
        return sumClosestToTarget;
    }
    
    // Time complexity: O(n^2)
}
```
