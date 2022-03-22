# Longest Substring with At Most K Distinct Characters

Given a string s and an integer k, return the length of the longest substring of s that contains at most k distinct characters.

https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/

## My solution:

```Java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        //key, val -> char, number of occurrences in substring
        Map<Character, Integer> freqMapping = new HashMap<>();
        int maxLength = 0;
        int left = 0;
        int right = 0;
        
        while (right < s.length()) {
            char c = s.charAt(right++);
            freqMapping.put(c, freqMapping.getOrDefault(c, 0) + 1);

            while (freqMapping.size() > k) {
                char leftmostChar = s.charAt(left++);
                int updatedOccurrences = freqMapping.get(leftmostChar) - 1;
                if (updatedOccurrences == 0) {
                    freqMapping.remove(leftmostChar);
                }
                else {
                    freqMapping.put(leftmostChar, updatedOccurrences);
                }
            }

            int currentLength = right-left;
            maxLength = Math.max(currentLength, maxLength);
        }
        
        return maxLength;
    }
}
```
