# Maximum Subarray

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

## Memory-inefficient python solution (lol, funny how this was once again the first solution I came up with; I'm pretty rusty at this)

```python3
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # We want a 2d array that can be used to represent the different partial sums we find
        # For example, sums[2][4] would refer to the sum of elements 2-4
        # If we want to find the value of sums[2][5], we would just need to do sums[2][4] + nums[5]
        numsLen = len(nums)
        subarraySums = [[None for _ in range(numsLen)] for _ in range(numsLen)]

        maxSum = nums[0]
    
        for i in range(0, numsLen):
            for j in range(i, numsLen):
                if i == j:
                    subarraySums[i][i] = nums[i]
                    maxSum = max(maxSum, subarraySums[i][i])
                else:
                    subarraySums[i][j] = subarraySums[i][j-1] + nums[j]
                    maxSum = max(maxSum, subarraySums[i][j])

        return maxSum
```

## My solution:

```Java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums.length == 0)
            return 0;
        
        int max = nums[0];
        int maxSoFar = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            // maxSoFar represents the sum of the subarray preceding nums[i].
            // If maxSoFar is negative, then we should start counting a new
            // subarray starting from nums[i], because extending said subarray
            // to include maxSoFar would only decrease the sum we could calculate from nums[i].
            maxSoFar = Math.max(maxSoFar + nums[i], nums[i]);
            max = Math.max(max, maxSoFar);
        }
        
        return max;
    }
}
```

## My second(?) attempt:

```Java
class Solution {
    public int maxSubArray(int[] nums) {
        int len = nums.length;
        if (len == 0) return 0;
        //sums[3] means the maximum sum that goes up to nums[3] (starting point can be anything <= 3)
        int[] sums = new int[len];
        
        int max = sums[0] = nums[0];
        
        for (int i = 1; i < len; i++) {
            //if sums[i-1] < 0 then that means the maximum sum that goes up to nums[i] so far will
            //exclude nums[0:i-1], because adding nums[0]+...+nums[i-1] to nums[i] will only decrease it
            if (sums[i-1] < 0) sums[i] = 0;
            else sums[i] = sums[i-1];
            sums[i] += nums[i];
            if (sums[i] > max) max = sums[i];
        }
        return max;
    }
}
```

## My first attempt:

```Java
class Solution {
    public int maxSubArray(int[] nums) {
        /*So this solution works afaik but it's very very inefficient memory-wise so it's kinda worthless 
        as a result, on leetcode I exceeded the memory limit on one of the test cases.*/
        //this 2D array will contain all of the possible sums like so:
        //sums[0][0] contains just nums[0]
        //sums[0][1] contains nums[0] + nums[1]
        //sums[1][3] contains nums[1] + nums[2] + nums[3]
        //etc
        int len = nums.length;
        if (len == 0) return 0;
        int[][] sums = new int[len][len];
        
        //initialize sums with the solutions for just one element
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < len; i++) {
            sums[i][i] = nums[i];
            if (sums[i][i] > max) {
                max = sums[i][i];
            }
        }
        
        for (int i = 0; i < len; i++) {
            for (int k = i+1; k < len; k++) {
                sums[i][k] = sums[i][k-1] + nums[k];
                if (sums[i][k] > max) {
                    max = sums[i][k];
                }
            }
        }
        return max;
    }
}
```
