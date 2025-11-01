# Valid Parentheses

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.

2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

## My python solution:

```python3
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for c in s:
            if c == '(' or c == '{' or c == '[':
                stack.append(c)
            else:
                if not stack:
                    return False
                
                if c == ')' and stack[-1] != '(':
                    return False
                if c == '}' and stack[-1] != '{':
                    return False
                if c == ']' and stack[-1] != '[':
                    return False
                
                stack.pop()
        
        return not stack
```

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

## My better solution:

```Java
class Solution {
    private static HashMap<Character, Character> bracketMappings = new HashMap<Character, Character>();
    static {
        bracketMappings.put('(', ')');
        bracketMappings.put('[', ']');
        bracketMappings.put('{', '}');
    }
    
    private boolean isValidClosedBacket(Stack<Character> stack, Character closedBracket) {
        if (stack.isEmpty()) {
            return false;
        }
        Character openBracket = stack.peek();
        Character validClosedBracket = bracketMappings.get(openBracket);
        return closedBracket.equals(validClosedBracket);
    }
    
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<Character>();
        for (Character c : s.toCharArray()) {
            if (bracketMappings.containsKey(c)) {
                stack.push(c);
            }
            else if (isValidClosedBacket(stack, c)) {
                stack.pop();
            }
            else {
                return false;
            }
        }
        return stack.isEmpty();
    }
}
```
