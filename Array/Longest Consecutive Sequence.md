# Longest Consecutive Sequence

Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

https://leetcode.com/problems/longest-consecutive-sequence/

## My solution:

```Java
class Solution {
    public int longestConsecutive(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        for (int num : nums)
            set.add(num);
        
        int maxLength = 0;
        
        for (int num : set) {
            //If num-1 exists in the set, then a longer consecutive sequence would start
            //from there, so we can ignore the current starting point.
            if (!set.contains(num-1)) {
                int currentLength = 1;
                
                //Count how long a consecutive sequence starting from this number is.
                while (set.contains(++num)) {
                    currentLength++;
                }
                
                maxLength = Math.max(maxLength, currentLength);
                
            }
        }
        
        return maxLength;
    }
    
    // Time complexity: O(n). The reason it's in linear time is because the inner while loop
    // is only run when the current number being processed marks the beginning of a sequence,
    // meaning that each sequence in the HashSet is iterated over only once inside that while
    // loop. The for loop also iterates over each element once, so the time complexity is
    // O(2n) or O(n).
    // Space complexity: O(n).
    
}
```
