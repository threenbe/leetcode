# Substring with Concatenation of All Words

You are given a string s and an array of strings words of the same length. Return all starting indices of substring(s) in s that is a concatenation of each word in words exactly once, in any order, and without any intervening characters.

You can return the answer in any order.

https://leetcode.com/problems/substring-with-concatenation-of-all-words/

## My slow as hell solution:

```Java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> results = new ArrayList<>();
        int wordSize = words[0].length();
        if (wordSize > s.length()) {
            return results;
        }
        
        int numWords = words.length;
        int left = 0;
        int right = wordSize * numWords;
        
        while (right <= s.length()) {
            int numWordsFound = 0;
            List<String> wordList = new ArrayList<String>(Arrays.asList(words));
            for (int i = left; i < right; i += wordSize) {
                String substr = s.substring(i, i + wordSize);
                boolean found = false;
                for (int j = 0; j < wordList.size(); j++) {
                    String word = wordList.get(j);
                    if (word.equals(substr)) {
                        found = true;
                        wordList.remove(j);
                        break;
                    }
                }
                
                if (found)
                    numWordsFound++;
                else
                    break;
            }
            
            if (numWordsFound == numWords) {
                results.add(left);
            }
            left++;
            right++;
        }
        
        return results;
    }
}
```
