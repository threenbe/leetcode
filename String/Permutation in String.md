# Permutation in String

Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.

In other words, return true if one of s1's permutations is the substring of s2.

## My solution:

```Java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        // The question is basically asking whether or not there is a substring
        // within s2 that contains exactly the same characters as s1, even
        // if they're out of order.
        
        // key, val -> char, the number of times this char appears in the string
        Map<Character, Integer> charFreq1 = new HashMap<>();
        for (char c : s1.toCharArray()) {
            charFreq1.put(c, charFreq1.getOrDefault(c, 0) + 1);
        }
        int left = 0;
        int right = s1.length();
        
        while (right <= s2.length()) {
            String substring = s2.substring(left, right);
            boolean foundPermutation = true;
            Map<Character, Integer> charFreq2 = new HashMap<>();
            for (char c : substring.toCharArray()) {
                charFreq2.put(c, charFreq2.getOrDefault(c, 0) + 1);
                if (!charFreq1.containsKey(c) || charFreq2.get(c) > charFreq1.get(c)) {
                    foundPermutation = false;
                    break;
                }
            }
            if (foundPermutation) {
                return true;
            }
            left++;
            right++;
        }
        
        return false;
    }
}
```
