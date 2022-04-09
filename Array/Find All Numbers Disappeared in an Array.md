# Find All Numbers Disappeared in an Array

Given an array nums of n integers where nums[i] is in the range [1, n], return an array of all the integers in the range [1, n] that do not appear in nums.

Follow up: Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/

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
