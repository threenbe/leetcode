# Longest Substring Without Repeating Characters

Given a string s, find the length of the longest substring without repeating characters.

## Python solution (wow, I went for the least efficient of my old solutions; I sure am rusty)

```python3
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        sLen = len(s)
        # This essentially tracks the current no-dupe substring
        charSet = set()
        i = 0
        j = 0
        maxLen = 0
        # abcabcbbdcfg
        #        i    j
        # i = 7, j = 12
        # set: b, d, c, f, g
        # currentLen = 5
        # maxLen = 5
        while j < sLen and i < sLen:
            if s[j] not in charSet:
                # Haven't run into a dupe yet, so advance substring length
                charSet.add(s[j])
                j += 1
                # Check if this is the longest substring with no dupes so far
                if (j-i) > maxLen:
                    maxLen = j-i
            else:
                # Now that there's a dupe somewhere in the substring, we advance
                # the front of the sliding window until there are no more dupes,
                # then we can keep checking to see a longer no-dupe substring can
                # form later down the line
                charSet.remove(s[i])
                i += 1
        return maxLen
```

## My solution:

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
            // At first I thought that I needed to delete chars that are no longer
            // part of our window, but as long as we know where our current window
            // starts, there's no need to as we know that we can just ignore all
            // chars before the start of our current window. This cuts down on some
            // unnecessary operations.
            if (chars.containsKey(c)) {
                int newLeft = chars.get(c)+1;
                left = Math.max(newLeft, left);
            }
            chars.put(c, right++);
            maxLength = Math.max(maxLength, right-left);
        }
        
        return maxLength;
    }
}
```

## Another solution:

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

## My initial solution:

```Java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int ret = 0;
        int i = 0; int j = 0;
        HashSet<Character> hs = new HashSet<Character>();
        while (i < s.length() && j < s.length()){
            if (!hs.contains(s.charAt(j))){
                hs.add(s.charAt(j++));
                if (ret < j-i){
                    ret = j-i;
                }
            } else {
                hs.remove(s.charAt(i++));
            }
        }
        return ret;
    }
}
```
