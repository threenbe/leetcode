# Find All Duplicates in an Array

Given an integer array nums of length n where all the integers of nums are in the range [1, n] and each integer appears once or twice, return an array of all the integers that appears twice.

You must write an algorithm that runs in O(n) time and uses only constant extra space.

## O(1) space solution that restores the input array after:
```Java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        // O(1) space solution that restores the input array after the fact.
        // We can indicate that a number has appeared once by marking its
        // corresponding index as negative, and we can indicate that it's
        // appeared twice by marking its corresponding index as positive.
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            int index = Math.abs(nums[i]) - 1; // corresponding index
            nums[index] *= -1;
        }
        
        // build solution list
        // make sure to skip indices of elements that DON'T appear in the input, since
        // these will also be positive
        List<Integer> duplicates = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int index = Math.abs(nums[i]) - 1;
            if (nums[index] > 0) {
                duplicates.add(index+1);
                nums[index] *= -1;
            }
        }
        
        // restore the input array
        for (int i = 0; i < n; i++) {
            nums[i] = Math.abs(nums[i]);
        }
        
        return duplicates;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(1)
}
```

## Cyclic sort solution:

```Java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        // Cyclic sort to place every element where it belongs.
        // Duplicates will end up in the wrong indices and can be sniffed out that way.
        // Since there's only one duplicate per element at most, we don't have to worry
        // about reporting the same duplicate more than once in the output.
        int n = nums.length, i = 0;
        while (i < n) {
            int j = nums[i]-1; // index j is where nums[i] should be
            if (nums[i] != nums[j])
                swap(nums, i, j);
            else
                i++;
        }
        
        List<Integer> duplicates = new ArrayList<>();
        for (i = 0; i < n; i++) {
            if (nums[i] != i+1)
                duplicates.add(nums[i]);
        }
        
        return duplicates;
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
    // Time complexity: O(n)
    // Space complexity: O(1)
}
```
