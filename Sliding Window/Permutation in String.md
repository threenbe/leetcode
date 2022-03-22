# Permutation in String

Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.

In other words, return true if one of s1's permutations is the substring of s2.

## My solution:

Instead of re-generating and re-populating the second character mapping array over and over for every substring of s2, we can just place our window at the first substring of s2, and then remove one preceding character and add one subsequent character, effectively sliding our window forward by 1 and receiving the next substring.

```Java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        // The question is basically asking whether or not there is a substring
        // within s2 that contains exactly the same characters as s1, even
        // if they're out of order.
        
        if (s1.length() > s2.length()) {
            return false;
        }
        
        // the number of times each char appears in the string
        int[] charFreq1 = new int[26];
        int[] charFreq2 = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            charFreq1[s1.charAt(i) - 'a']++;
            charFreq2[s2.charAt(i) - 'a']++;
        }
        int left = 0;
        int right = s1.length();
        
        while (right < s2.length()) {
            if (mappingsAreEqual(charFreq1, charFreq2)) {
                return true;
            }
            //else, advance the window forward
            charFreq2[s2.charAt(left++) - 'a']--;
            charFreq2[s2.charAt(right++) - 'a']++;
        }
        
        // We don't check for a match on the last iteration because we break
        // out of the loop after updating the maps, so check it here.
        return mappingsAreEqual(charFreq1, charFreq2);
    }
    
    private boolean mappingsAreEqual(int[] a1, int a2[]) {
        for (int i = 0; i < 26; i++) {
            if (a1[i] != a2[i]) {
                return false;
            }
        }
        return true;
    }
}
```

## My (still kinda slow) solution:

When we need to track character counts, it's a lot faster to index into an array than to update a HashMap, which requires a hashing function, checking whether the key exists, etc.

```Java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        // The question is basically asking whether or not there is a substring
        // within s2 that contains exactly the same characters as s1, even
        // if they're out of order.
        
        // the number of times each char appears in the string
        int[] charFreq1 = new int[26];
        for (char c : s1.toCharArray()) {
            charFreq1[c - 'a']++;
        }
        int left = 0;
        int right = s1.length();
        
        while (right <= s2.length()) {
            String substring = s2.substring(left, right);
            boolean foundPermutation = true;
            int[] charFreq2 = new int[26];
            for (char c : substring.toCharArray()) {
                charFreq2[c - 'a']++;
                if (charFreq2[c - 'a'] > charFreq1[c - 'a']) {
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

## My (very slow) solution:

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
