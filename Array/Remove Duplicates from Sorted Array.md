# Remove Duplicates from Sorted Array

Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

https://leetcode.com/problems/remove-duplicates-from-sorted-array/

## My solution:

```Java
class Solution {
    public int removeDuplicates(int[] nums) {
        // Remove duplicates from nums.
        // When we remove an element, we have to shift every element after it back.
        // The ideal solution will ensure that every element that needs to be shifted
        // back is only shifted once.
        
        int slow = 0;
        int fast = 1;
        
        while (fast < nums.length) {
            if (nums[slow] != nums[fast]) {
                // nums[fast] is not a duplicate of a previous value
                slow++;
                nums[slow] = nums[fast];
            }
            // If nums[fast] IS a duplicate value, then we just increment our fast
            // pointer but not our slow one. Eventually a non-duplicate value will
            // be added to the slow index in which nums[fast] would have been added
            // had it not been a duplicate, effectively overwriting the duplicate.
            fast++;
        }
        return slow + 1;
        
        // An example:
        // nums = [1,1,2]
        // slow = 0, fast = 1
        // nums[slow] == nums[fast] -> slow = 0, fast = 2
        // nums[slow] != nums[fast] -> slow = 1, fast = 2, nums = [1,2,2]
        // slow = 1, fast = 3; break out of loop
        // slow is pointing to the last index of our "new," add 1 to get the length
        // -> [1,2 (ignored), 2], return length 2.
        // If we had e.g. nums = [1,1,2], then we'd go from [1,2,2,3] -> [1,2,3,3]
        // and return a length of 3.
    }
}
```
