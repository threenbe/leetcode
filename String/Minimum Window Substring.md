# Minimum Window Substring

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

A substring is a contiguous sequence of characters within the string.

https://leetcode.com/problems/minimum-window-substring/

## My solution:

```Java
class Solution {
    public String minWindow(String s, String t) {
        // Find the smallest window inside s that contains all
        // the characters inside t, including duplicates (i.e.
        // if t has two A's, then there should be two A's in the window).
        
        if (t.length() > s.length()) {
            return "";
        }
        
        int[] charFreqT = new int[128];
        int[] charFreqS = new int[128];
        for (int i = 0; i < t.length(); i++) {
            charFreqT[t.charAt(i)]++;
            charFreqS[s.charAt(i)]++;
        }
        
        int left = 0;
        int right = t.length();
        int minSubstrLeft = 0;
        int minSubstrRight = 0;
        int minLength = Integer.MAX_VALUE;
        
        while (right < s.length()) {
            if (right-left >= t.length() && isWindowSubstring(charFreqT, charFreqS)) {
                if (minLength > right-left) {
                    minLength = right-left;
                    if (minLength == t.length()) {
                        return s.substring(left, right);
                    }
                    minSubstrLeft = left;
                    minSubstrRight = right;
                }
                // Shrink window to see if we can find a smaller one
                charFreqS[s.charAt(left++)]--;
            }
            else {// Expand window to see if we can find a valid one
                charFreqS[s.charAt(right++)]++;
            }
        }
        
        // Evaluate the last window in which right == s.length()-1
        while (right-left >= t.length() && isWindowSubstring(charFreqT, charFreqS)) {
            if (minLength > right-left) {
                minLength = right-left;
                minSubstrLeft = left;
                minSubstrRight = right;
            }
            charFreqS[s.charAt(left++)]--;
        }
        
        return minLength == Integer.MAX_VALUE ? "" : s.substring(minSubstrLeft, minSubstrRight);
    }
    
    private boolean isWindowSubstring(int[] charFreq1, int[] charFreq2) {
        for (int i = 0; i < 128; i++) {
            if (charFreq1[i] > 0 && charFreq1[i] > charFreq2[i]) {
                return false;
            }
        }
        return true;
    }
}
```
