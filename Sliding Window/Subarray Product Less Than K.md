# Subarray Product Less Than K

Given an array of integers nums and an integer k, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than k.

https://leetcode.com/problems/subarray-product-less-than-k/

## My solution:

```Java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int left = 0;
        int right = 0;
        int currentProduct = 1;
        int numOfSubarrays = 0;
        while (left < nums.length) {
            currentProduct *= nums[right++];
            if (currentProduct < k) {
                numOfSubarrays++;
                if (right == nums.length) {
                    //reset the window
                    currentProduct = 1;
                    right = ++left;
                }
            }
            else {
                //reset the window
                currentProduct = 1;
                right = ++left;
            }
        }
        return numOfSubarrays;
    }
}
```
