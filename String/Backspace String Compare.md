# Backspace String Compare

Given two strings s and t, return true if they are equal when both are typed into empty text editors. '#' means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

https://leetcode.com/problems/backspace-string-compare/

## My solution:

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
