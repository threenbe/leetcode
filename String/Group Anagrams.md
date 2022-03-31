# Group Anagrams

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

https://leetcode.com/problems/group-anagrams/

## My solution:

```Python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        '''Map each string to a character count representation.
        If two strings are anagrams of each other, then they'll
        be mapped to the same character count representation.'''
        
        anagram_mapping = {}
        for s in strs:
            char_count = [0] * 26
            for char in s:
                char_count[ord(char) - ord('a')] += 1
            string_list =  anagram_mapping.get(tuple(char_count), [])
            string_list.append(s)
            anagram_mapping[tuple(char_count)] = string_list
        return anagram_mapping.values()
    
    # Time complexity: O(n*k) where is the number of strings, and k is the length
    # of each string. We iterate over the list of strings (O(n)) and then we iterate 
    # over each individual string to count its characters (O(k)).
    # Space complexity: O(n*k)
```

## My initial solution:

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
