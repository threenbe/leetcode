# Sort Colors

Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

## My solution:

```Java
class Solution {
    public void sortColors(int[] nums) {
        // Array contains 0s, 1s, and 2s.
        // Sort it so that the numbers are in order.
        if (nums.length == 0)
            return;
        
        // key, val -> value, number of occurrences
        Map<Integer, Integer> occurrences = new HashMap<>();
        for (Integer n : nums) {
            occurrences.put(n, occurrences.getOrDefault(n, 0) + 1);
        }
        int numReds = occurrences.getOrDefault(0, 0);
        int numWhites = occurrences.getOrDefault(1, 0);
        int numBlues = occurrences.getOrDefault(2, 0);
        
        int i;
        for (i = 0; i < numReds; i++) {
            nums[i] = 0;
        }
        for (; i < numReds+numWhites; i++) {
            nums[i] = 1;
        }
        for (; i < nums.length; i++) {
            nums[i] = 2;
        }
    }
}
```
