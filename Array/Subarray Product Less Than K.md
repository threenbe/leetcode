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
        // ex: [10,5,2,6], k = 100
        // if e.g. [5,2,6] is a valid subarray, then we know that all of its subarrays
        // are also valid subarrays.
        // Suppose we start a window at [10]. We add +1. Then expand to [10,5]. This contains
        // three valid subarrays -- [10], [10,5], and [5]. Since we already counted [10], we
        // add +2 here. Incidentally, this is also the length of the current subarray.
        // Then we expand to [10,5,2] which breaks our rule. Reset the window to [5].
        // Note that we've already counted [5] as a subarray so don't count it again.
        // Our next subarray should be [5,2] and it should only add +2 to our count for [5,2]
        // and [2].
        while (right < nums.length) {
            currentProduct *= nums[right++];
            while (left < right && currentProduct >= k) {
                currentProduct /= nums[left++];
            }
            numOfSubarrays += right-left;
        }
        return numOfSubarrays;
    }
}
```

## My initial solution:

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
