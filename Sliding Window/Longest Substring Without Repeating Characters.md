# Longest Substring Without Repeating Characters

Given a string s, find the length of the longest substring without repeating characters.

## My solution

```Java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // key, val -> char, index of occurrence
        Map<Character, Integer> chars = new HashMap<>();
        int left = 0;
        int right = 0;
        int maxLength = 0;
        
        while (right < s.length()) {
            char c = s.charAt(right);
            // When we find a char that appears in our current substring,
            // we advance the start of our window to just after its first appearance.
            if (chars.containsKey(c)) {
                int newLeft = chars.get(c)+1;
                while (left < newLeft) {
                    chars.remove(s.charAt(left++));
                }
            }
            chars.put(c, right++);
            maxLength = Math.max(maxLength, right-left);
        }
        
        return maxLength;
    }
}
```
