# Valid Parentheses

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

## My solution:

```Java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> open_brackets = new Stack<Character>();
        
        for (int i = 0; i < s.length(); i++) {
            Character c = s.charAt(i);
            if (c.equals(')')) {
                if (open_brackets.empty()) return false;
                if (open_brackets.peek().equals('(')) {
                    open_brackets.pop();
                }
                else return false;
            }
            else if (c.equals('}')) {
                if (open_brackets.empty()) return false;
                if (open_brackets.peek().equals('{')) {
                    open_brackets.pop();
                }
                else return false;
            }
            else if (c.equals(']')) {
                if (open_brackets.empty()) return false;
                if (open_brackets.peek().equals('[')) {
                    open_brackets.pop();
                }
                else return false;
            }
            else if (c.equals('(') || c.equals('[') || c.equals('{')) {
                open_brackets.push(c);
            }
        }
        
        if (!open_brackets.empty()) return false;
        return true;
    }
}
```