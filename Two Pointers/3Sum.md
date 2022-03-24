# 3Sum

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

## My solution:

```Java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        // Return all triplets such that the elements in each triplet adds up to 0.
        // The solution set must not have duplicate triplets.
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length && nums[i] <= 0; i++) {
            // If nums[i] is a duplicate of nums[i-1] then skip it,
            // we've already evaluated triplets including it
            if (i == 0 || nums[i-1] != nums[i]) {
                twoSum(nums, i, result);
            }
        }
        
        return result;
    }
    
    private void twoSum(int[] nums, int i, List<List<Integer>> result) {
        int target = -nums[i]; // nums[i] + two more nums will equal 0
        int left = i+1;
        int right = nums.length-1;
        while (left < right) {
            int sum = nums[left] + nums[right];
            if (sum == target) {
                result.add(Arrays.asList(nums[i], nums[left++], nums[right--]));
                // Skip to the next non-duplicate nums[left] value 
                // to avoid creating duplicate triplets.
                while (left < right && nums[left-1] == nums[left]) {
                    left++;
                }
            }
            else if (sum < target) {
                left++;
            }
            else {
                right--;
            }
        }
    }
}
```
