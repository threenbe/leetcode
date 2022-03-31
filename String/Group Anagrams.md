# Group Anagrams

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

https://leetcode.com/problems/group-anagrams/

## My solution

```Java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs.length == 0)
            return null;
        
        // key, val -> sorted anagram, all words that are an anagram of the key
        Map<String, List<String>> anagramMapping = new HashMap<>();
        
        for (String str : strs) {
            char[] sortedCharArr = str.toCharArray();
            Arrays.sort(sortedCharArr);
            String sortedStr = new String(sortedCharArr);
            List<String> list = anagramMapping.getOrDefault(sortedStr, new ArrayList<String>());
            list.add(str);
            anagramMapping.put(sortedStr, list);
        }
        
        return new ArrayList<List<String>>(anagramMapping.values());
    }
    
    // Time complexity: O(n * k*log(k)) where n is the number of strings and k is the length
    // of each string. We iterate through every string (O(n)) and sort them (O(k*log(k))).
    // Space complexity: O(n*k)
}
```
