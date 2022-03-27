# Backspace String Compare

Given two strings s and t, return true if they are equal when both are typed into empty text editors. '#' means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

https://leetcode.com/problems/backspace-string-compare/

## My solution:

```Java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        // Use two pointers that point to the end of each word.
        // Decrement the pointers one-by-one and compare each
        // character. When you come across a '#', skip it as well
        // as the next character. If you come across multiple '#'s
        // in a row, then skip that many subsequent characters.
        int sPtr = s.length()-1;
        int tPtr = t.length()-1;
        
        while (sPtr >= 0 || tPtr >= 0) {
            int skipCount = 0;
            while (sPtr >= 0) {
                if (s.charAt(sPtr) == '#') {
                    sPtr--;
                    skipCount++;
                }
                else if (skipCount > 0) {
                    sPtr--;
                    skipCount--;
                }
                else {
                    break;
                }
            }
            
            skipCount = 0;
            while (tPtr >= 0) {
                if (t.charAt(tPtr) == '#') {
                    tPtr--;
                    skipCount++;
                }
                else if (skipCount > 0) {
                    tPtr--;
                    skipCount--;
                }
                else {
                    break;
                }
            }
            
            // compare characters
            if (sPtr >= 0 && tPtr >= 0) {
                if (s.charAt(sPtr) != t.charAt(tPtr)) {
                    return false;
                }
            }
            else if (sPtr >= 0 || tPtr >= 0) {//if one is 0 or less but the other isn't
                return false;
            }
            sPtr--;
            tPtr--;
        }
        
        return true;
    }
    
    // Time complexity: O(n)
    // Space complexity: O(1)
}
```

## My initial solution:

```Java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        String typedS = constructString(s);
        String typedT = constructString(t);
        return typedS.equals(typedT);
    }
    
    private String constructString(String s) {
        Stack<Character> stack = new Stack<>();
        for (Character c : s.toCharArray()) {
            if (c == '#') {
                if (!stack.isEmpty()) {
                    stack.pop();
                }
            }
            else {
                stack.push(c);
            }
        }
        return stack.toString();
        /*StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop());
        }
        return sb.toString();*/
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n)
}
```
