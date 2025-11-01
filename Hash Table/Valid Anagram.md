# Valid Anagram

Given two strings `s` and `t`, return `true` if `t` is an of `s`, and `false` otherwise.

## My solution:

```python3
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        chars = dict()
        for c in s:
            if c in chars:
                chars[c] += 1
            else:
                chars[c] = 1

        for c in t:
            if c not in chars:
                return False
            chars[c] -= 1
            if chars[c] == 0:
                chars.pop(c)
        
        # If map is empty then it's an anagram, else it's not
        return not chars
```
