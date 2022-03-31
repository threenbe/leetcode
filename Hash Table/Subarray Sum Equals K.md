# Subarray Sum Equals K

Given an array of integers nums and an integer k, return the total number of subarrays whose sum equals to k.

https://leetcode.com/problems/subarray-sum-equals-k/

## My solution:

```Java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int sum = 0;
        // key, val -> prefix sum, number of times this sum has occurred
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            sum += num;
            // check the subarray that starts from the beginning
            if (sum == k)
                count++;
            // check the number of subarrays starting from other indices that add up to k
            count += map.getOrDefault(sum - k, 0);
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
