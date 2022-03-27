# Sort Colors

Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

# My solution:

```Java
class Solution {
    public void sortColors(int[] nums) {
        // Array contains 0s, 1s, and 2s.
        // Sort it so that the numbers are in order.
        // The constaint here, as obvious as it sounds,
        // is that all the 0s will be on the left and all
        // the 2s will be on the right. So a 0 that is "out
        // of bounds" so to speak should be moved back within
        // bounds to the left, and same for 2s that are too far left.
        if (nums.length == 0)
            return;
        
        int left = 0;
        int curr = 0;
        int right = nums.length - 1;
        
        while (curr <= right) {
            if (nums[curr] == 0) {
                //move to the left
                //if it's already in the left, cool
                swap(nums, left++, curr++);
            }
            else if (nums[curr] == 2) {
                //move to the right
                //if it's already in the right, cool
                //don't move curr pointer forward just yet, we might
                //need to operate on the value we just moved to the left
                swap(nums, curr, right--);
            }
            else {//if nums[curr] == 1
                curr++;
            }
        }
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

## My initial solution:

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
