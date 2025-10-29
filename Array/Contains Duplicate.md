# Contains Duplicate

Given an integer array nums, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

https://leetcode.com/problems/contains-duplicate/description/

## My solution:

```python3
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        numSet = set()
        # [1, 2, 3, 2]
        for num in nums:
            if num in numSet:
                return True
            numSet.add(num)
        return False
```
