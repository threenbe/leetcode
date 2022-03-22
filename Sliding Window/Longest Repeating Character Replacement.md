# Longest Repeating Character Replacement

You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

https://leetcode.com/problems/longest-repeating-character-replacement/

## Slightly optimized solution:

```Java
class Solution {
    public int characterReplacement(String s, int k) {
        // Find the longest substring in which every character is
        // the same, with the exception of 'k' characters at most.
        // In other words, the substring can have up to k+1 distinct
        // characters. The most frequently occurring character can appear
        // as many times as it wants, but the total count of all the other
        // characters cannot exceed k.
        
        // key, val -> char, num of occurrences in substring
        Map<Character, Integer> map = new HashMap<>();
        int left = 0;
        int right = 0;
        int highestFreq = 0;
        int maxLength = 0;
        
        // We start with a window of size 1 and expand it as long as the substring 
        // contained within it is met. Once the condition is broken (i.e. there are 
        // more than 'k' characters that aren't the most frequently occurring one), then
        // we shift the entire window forward until the substring inside it is valid again.
        while (right < s.length()) {
            char c = s.charAt(right++);
            map.put(c, map.getOrDefault(c, 0) + 1);
            int currentLength = right-left;
            highestFreq = Math.max(highestFreq, map.get(c));
            
            if (currentLength - highestFreq > k) {
                char leftmostChar = s.charAt(left++);
                map.put(leftmostChar, map.get(leftmostChar) - 1);
                currentLength = right-left;
            }
            
            maxLength = Math.max(maxLength, currentLength);
        }
        
        return maxLength;
    }
}
```

## My solution:

```Java
class Solution {
    public int characterReplacement(String s, int k) {
        // Find the longest substring in which every character is
        // the same, with the exception of 'k' characters at most.
        // In other words, the substring can have up to k+1 distinct
        // characters. The most frequently occurring character can appear
        // as many times as it wants, but the total count of all the other
        // characters cannot exceed k.
        
        // key, val -> char, num of occurrences in substring
        Map<Character, Integer> map = new HashMap<>();
        int left = 0;
        int right = 0;
        int highestFreq = 0;
        int maxLength = 0;
        
        while (right < s.length()) {
            char c = s.charAt(right++);
            map.put(c, map.getOrDefault(c, 0) + 1);
            int currentLength = right-left;
            highestFreq = Math.max(highestFreq, map.get(c));
            
            while (currentLength - highestFreq > k) {
                char leftmostChar = s.charAt(left++);
                map.put(leftmostChar, map.get(leftmostChar) - 1);
                highestFreq = 0;
                for (int n : map.values()) {
                    highestFreq = Math.max(highestFreq, n);
                }
                currentLength = right-left;
            }
            
            maxLength = Math.max(maxLength, currentLength);
        }
        
        return maxLength;
    }
}
```
