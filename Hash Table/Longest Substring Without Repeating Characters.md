# Longest Substring Without Repeating Characters

Given a string, find the length of the longest substring without repeating characters.

## My solution:

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