# Valid Anagram

Given two strings `s` and `t`, return `true` if `t` is an of `s`, and `false` otherwise.

## More elegant solution:

```python3
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        chars = dict()

        for c in s:
            chars[c] = chars.get(c, 0) + 1
        
        for c in t:
            if c not in chars or chars[c] == 0:
                return False
            chars[c] -= 1

        # Originally I thought that you had to check if the map was empty or not at this point.
        # However, given that we've determined that the two input strings are of equal length at
        # this point, it's impossible to end up here with a non-empty map. In order for that to
        # happen, there would have to be a character from "s" that is not in "t", and since "s"
        # and "t" are of equal length, this means that "t" has a unique character, which would always
        # trigger the if block to return False. If we never hit that if block, then it's because "s"
        # and "t" are of equal lengths and have the exact same characters.
        return True
```

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
