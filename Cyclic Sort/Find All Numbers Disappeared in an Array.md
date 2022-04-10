# Find All Numbers Disappeared in an Array

Given an array nums of n integers where nums[i] is in the range [1, n], return an array of all the integers in the range [1, n] that do not appear in nums.

Follow up: Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/

## Solution using O(1) space that restores the input array after:

```Java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        // O(1) space solution that restores the input array after.
        // Mark values that we've seen by marking their corresponding indices
        // as negative. After we've found our solution, we can easily revert
        // the array back.
        for (int i = 0; i < nums.length; i++) {
            int index = Math.abs(nums[i])-1;
            nums[index] = Math.abs(nums[index]) * -1;
        }
        
        // build output list
        List<Integer> output = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0)
                output.add(i+1);
        }
        
        // restore input array
        for (int i = 0; i < nums.length; i++) {
            nums[i] = Math.abs(nums[i]);
        }
        
        return output;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(1)
}
```

## Cyclic sort solution:

```Java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        // Cyclic sort solution
        int n = nums.length;
        int i = 0;
        while (i < n) {
            int j = nums[i]-1; // If, say, nums[i] is 5, then we calculate 5's correct index as 4
            // Check if nums[i] is indeed where it should be (note that this won't swap 
            // duplicates, which we want, otherwise we'd be stuck swapping them forever)
            if (nums[i] != nums[j])
                swap(nums, i, j);
            else
                i++;
        }
        
        List<Integer> output = new ArrayList<>();
        for (i = 0; i < n; i++) {
            if (nums[i] != i+1)
                output.add(i+1);
        }
        
        return output;
    }
    
    private void swap(int[] nums, int i1, int i2) {
        int tmp = nums[i1];
        nums[i1] = nums[i2];
        nums[i2] = tmp;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(1)
}
```

## Solution with extra space:

```Java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        // Quick solution that uses extra space
        HashSet<Integer> numSet = new HashSet<>();
        for (int i = 0; i < nums.length; i++)
            numSet.add(nums[i]);
        
        List<Integer> missingNums = new ArrayList<>();
        for (int n = 1; n <= nums.length; n++)
            if (!numSet.contains(n))
                missingNums.add(n);
        
        return missingNums;
    }
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
