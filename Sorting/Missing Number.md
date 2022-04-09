# Missing Number

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

https://leetcode.com/problems/missing-number/

## Bit manipulation:

```Java
class Solution {
    public int missingNumber(int[] nums) {
        int missingNumber = 0;
        int n = nums.length;
        
        // XOR with every number that should be there.
        for (int i = 0; i <= n; i++)
            missingNumber ^= i;
        
        // Then filter out the numbers that are actually there.
        for (int i = 0; i < n; i++)
            missingNumber ^= nums[i];
        
        // You're left with the number that's missing.
        return missingNumber;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(1)
}
```

## Cyclic sort solution:

```Java
class Solution {
    public int missingNumber(int[] nums) {
        // Use cyclic sort.
        // Try to match every number with its index, except n.
        // The one that doesn't match by the end (i.e. the index
        // containing n) is the missing number. If every number
        // matches its index, then that means n is the missing
        // number.
        int n = nums.length;
        int i = 0;
        while (i < n) {
            if (nums[i] != n && nums[i] != i)
                swap(nums, i, nums[i]);
            else
                i++;
        }
        
        for (i = 0; i < n; i++) {
            if (nums[i] != i)
                return i;
        }
        
        return n;
    }
    
    private void swap(int[] nums, int idx1, int idx2) {
        int tmp = nums[idx1];
        nums[idx1] = nums[idx2];
        nums[idx2] = tmp;
    }
    
    // Time complexity: O(n). In the first loop, we iterate over the array and move each
    // element we encounter to its correct index if need be. There is no need to move it
    // again after it's been placed at the correct index, so this is done in linear time.
    // Then the second for loop just iterates over the array once, which is linear time.
    // Space complexity: O(1).
}
```
