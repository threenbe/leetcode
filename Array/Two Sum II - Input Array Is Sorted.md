# Two Sum II - Input Array Is Sorted

Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length.

Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/

(literally what is the point of 1-indexing the arrays and placing so much emphasis on that lol)

## Python solution:

```python3
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        leftIdx = 0
        rightIdx = len(numbers)-1
        solution = []
        # Start with two pointers at the beginning and end of the array respectively.
        # If the sum of these values is too large, then decrement the right pointer.
        # This will give us a smaller sum, given that the array is sorted in non-decreasing
        # order. Likewise, if the sum is too small, then increment the left pointer.
        # Eventually, we'll find the solution.

        while (leftIdx != rightIdx):
            currentSum = numbers[leftIdx] + numbers[rightIdx]
            if currentSum == target:
                solution.append(leftIdx + 1)
                solution.append(rightIdx + 1)
                break
            elif currentSum > target:
                rightIdx -= 1
            else:
                leftIdx += 1
        
        return solution
```

## My solution:

```Java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length-1;
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum == target) {
                return new int[]{left+1, right+1};
            }
            else if (sum < target) {
                left++;
            }
            else {
                right--;
            }
        }
        return new int[2];
    }
}
```
