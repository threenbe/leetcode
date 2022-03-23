# Squares of a Sorted Array

Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

## My solution:

```Java
class Solution {
    public int[] sortedSquares(int[] nums) {
        // The negative numbers in the array will be sorted in non-increasing order
        // after being squared, while the positive numbers will remain in non-decreasing
        // order. So it makes sense to construct the result array from the end, starting
        // with the largest squares and working our way down.
        int left = 0;
        int right = nums.length-1;
        int[] result = new int[nums.length];
        for (int i = nums.length-1; i >= 0; i--) {
            if (Math.abs(nums[left]) > Math.abs(nums[right])) {
                result[i] = nums[left]*nums[left++];
            }
            else {
                result[i] = nums[right]*nums[right--];
            }
        }
        return result;
        
        // Example:
        // input = [-4, -1, 0, 3, 10], result = [_, _, _, _, _]
        // |-4| < |10|; start with 10, result = [_, _, _, _, 10]
        // |-4| > |3|; take 4 next, result = [_, _, _, 16, 10]
        // |-1| < |3|, etc, you get the idea.
        
        // Time complexity: O(n); we operate on each value of the input array once.
    }
}
```
