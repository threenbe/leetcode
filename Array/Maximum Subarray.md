# Maximum Subarray

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

## Most efficient solution (I think) in python:

```python3
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # We want to start from the 0th value in the array, and calculate the max possible
        # sum of any subarray that ends at that 0th value (in this case, it's just the 0th
        # value on its own, obviously).
        # Then, we want to calculate the max possible value of any subarray that ends at the 1st value,
        # i.e., does the subarray of 0th val + 1st val have a higher value than the subarray of just
        # the 1st value? This is how we know whether or not to include the 0th value when calculating
        # subsequent subarray sums, because if the 0th value will only ever decrease the sum of 
        # subsequent subarrays, then there's no point in including it. In any case, after calculating
        # the max sum of any subarray that goes up to the 1st value (whichever subarray that may be),
        # we check to see if this is a higher sum than the max sum of any subarray that goes up to the
        # 0th value calculated previously. This is how we arrive at our newest max sum. However, even
        # if the value of just the 0th element is greater than the sum of 0th + 1st, we continue to 
        # expand our subarray to see if adding more values to 0th + 1st can result in a greater sum 
        # than just 0th. Hence the distinction between, let's say, "maxSum" and "maxSumOfCurrentSubarray."
        # Then, we want to calculate the max possible value of any subarray that ends at the
        # 2nd value, i.e., 0th + 1st + 2nd vs 1st + 2nd vs 2nd. In the previous step, we already
        # determined the max sum between 0th + 1st vs 1st, so the choice between 0th + 1st + 2nd
        # vs 1st + 2nd will essentially look like "maxSumOfCurrentSubarray" + 2nd. Then, we
        # have to decide whether or not just the 2nd value on its own is higher or not. If
        # "maxSumOfCurrentSubarray" is a negative number, then the *new* maximum subarray sum
        # is just the 2nd value on its own. Like before, we would decide to exclude the previous
        # subarray when calculating new subarrays, because including it will always reduce the value
        # of subsequent sums. But, otherwise, we would obviously include it (ergo, continue to expand
        # our current subarray rather than start a new one).
        # With this overly lengthy exercise in logic, we can see that the logic basically repeats
        # itself on each new element, as long as we track the sum of the subarray that we're currently
        # building. Whenever our current subarray sum is negative, we start over with a new subarray 
        # starting with the new value. It might be that our current subarray sum is the highest we'll
        # ever see, but if including it in subsequent sums could only ever possibly lower *those* sums,
        # it should be discarded.
        ###############################

        ###############################
        # Our first subarray is just the first element, and so it's also our first max sum
        maxSum = nums[0]
        maxSumOfCurrentSubarray = nums[0]

        for i in range(1, len(nums)):
            # What gives us a higher sum: expanding the current subarray, or starting a new one?
            maxSumOfCurrentSubarray = max(nums[i] + maxSumOfCurrentSubarray, nums[i])
            # After making a decision above, does the current subarray give us a higher sum than
            # that of whichever subarray gave us our previous maximum?
            maxSum = max(maxSum, maxSumOfCurrentSubarray)

        return maxSum
```

## Second python attempt:

```python3
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # sums[i] is equal to the sum of values from nums[x] to nums[i], where `x` is less than
        # `i` and where no value other than `x` could result in a higher sum when calculating
        # nums[x] + nums[x+1] + ... + nums[i-1] + nums[i].
        sums = [0] * len(nums)

        # The only possible sum for sums[0] is with the 0th element alone, so we just set that
        # first and then start our operation from the 1st element.
        sums[0] = nums[0]
        maxSum = sums[0]
        for i in range(1, len(nums)):
            sums[i] = nums[i]

            # If sums[i-1] is less than 0, then we exclude it going forward. This is because
            # the maximum sum calculated between nums[0:i-1] + nums[i] would be less than just
            # nums[i], so the maximum sum up to the ith index is just nums[i] alone.
            # Therefore, if sums[i] represents the maximum possible sum up to the ith value, it
            # must start from the ith value.
            # Let's observe with this example:
            # [3, -2, -4, 6, -1, 2]
            # From nums[0] to nums[0], the max sum is obviously just 3.
            # From nums[0] to nums[1], we see nums[1-1] > 0, so we add it. This implies that the max
            # sum of any subarray starting before nums[1] up to nums[1] is 1, which is in fact true.
            # For sums up to nums[2], the possible sums are -3, -6, and -4. Our logic allows us to
            # correctly arrive at -3, since sums[2-1] > 0, so we know that adding it to nums[2] will
            # give us a higher sums[2] value, and we're trusting that sums[1] is the max sum up to that
            # point as well.
            # For sums up to nums[3], the possible sums are 3, 0, 2, and 6. The maximum sum we found up
            # up to nums[2] was -3, and indeed, if we add this to 6, we end up with the highest possible
            # sum...excluding the scenario where we just take 6 on its own. Since we see that sums[2]
            # is a negative number, we know that the maximum possible sum up to nums[3], given any
            # starting point, is simply 6, where nums[3] itself is the starting point.
            # To keep the rest of this brief, sums[4] is 5 (which is the highest sum possible if we
            # go up to nums[4], but not the highest sum possible in general). Then, sums[5] is 7, which
            # is indeed the highest sum possible if we go up to nums[5], and also the highest sum
            # possible in general. If we were to try to include any previous values in sums[5],
            # we would end up with a lower value. 7-4 is less than 7. 7-4-2 is less than 7.
            # 7-4-2+3 is still less than 7. We already knew this, because sums[2] was negative, so
            # including it going forward would always reduce the value of any possible subarray sum we
            # could calculate.
            if sums[i-1] > 0:
                sums[i] += sums[i-1]

            maxSum = max(maxSum, sums[i])

        return maxSum
```

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
