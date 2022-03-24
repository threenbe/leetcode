# 3Sum Smaller

Given an array of n integers nums and an integer target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy the condition nums[i] + nums[j] + nums[k] < target.

## My solution:

```Java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        int count = 0;
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 2; i++) {
            int left = i+1;
            int right = nums.length-1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum < target) {
                    // In a sorted array nums with indices i, j, and k such that
                    // i < j < k and nums[i] + nums[j] + nums[k] < target, we
                    // can deduce that the sums of all triplets with indices
                    // i, j, and m where j < m < k will also be less than target.
                    // So there's no need to decrement the right pointer here to
                    // check the sums of those triplets, just add them all to the count.
                    count += right-left;
                    left++;
                }
                else {
                    right--;
                }
            }
        }
        return count;
    }
    
    // Time complexity: O(n^2)
}
```
