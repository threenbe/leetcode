# Find All Anagrams in a String

Given two strings s and p, return an array of all the start indices of p's anagrams in s. You may return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

https://leetcode.com/problems/find-all-anagrams-in-a-string/

## My solution:

```Java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        if (p.length() > s.length()) {
            return result;
        }
        
        // Slide a window of size p.length() across s.
        // For each substring in s, check to see if it's
        // an anagram of p. If it is, then add the window's
        // starting index in the result arraylist.
        
        int[] charFreqP = new int[26];
        int[] charFreqS = new int[26];
        for (int i = 0; i < p.length(); i++) {
            charFreqP[p.charAt(i) - 'a']++;
            charFreqS[s.charAt(i) - 'a']++;
        }
        
        int left = 0;
        int right = p.length();
        
        while (right < s.length()) {
            // check if the substring in the current window is an anagram
            if (Arrays.equals(charFreqP, charFreqS)) {
                result.add(left);
            }
            // slide window forward
            charFreqS[s.charAt(left++) - 'a']--;
            charFreqS[s.charAt(right++) - 'a']++;
        }
        
        // check the last window
        if (Arrays.equals(charFreqP, charFreqS)) {
            result.add(left);
        }
        
        return result;
    }
    
    // Time complexity: O(N) where N is proportional to s.length()
}
```
