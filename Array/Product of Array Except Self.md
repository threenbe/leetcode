# Product of Array Except Self

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Follow up**: Can you solve the problem in O(1) extra space complexity? (The output array does not count as extra space for space complexity analysis.)

## O(n) time complexity and O(n) space complexity:

```python3
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        answer = [1] * len(nums)
        # The value of answer[i] can be calculated as follows: take
        # the product of nums[:i] and the product of nums[i+1:], and
        # then multiply those together. This obviously is the product
        # of all nums expect nums[i]. This has to be done in such a way
        # that redundant calculations are avoided when calculating for
        # every answer[i] value.
        # Let's take an array left[] such that left[i] is the product of
        # nums[:i], and an array right[] such that right[i] is the product
        # of nums[i+1:].
        # Example array: [1,2,3,4]
        # e.g. left[1] would just be nums[0] = 1
        # then left[2] would be nums[0] * nums[1] = 1*2 = 2,
        #   OR, left[0] * nums[1] = 1*2 = 2
        # then left[3] would be nums[0] * nums[1] * nums[2] = 1*2*3 = 6,
        #   OR left[1] * nums[2] = 2*3 = 6
        # left[0] would be nothing, since there is nothing before the 0th index;
        #   probably safe to just stick a 1 there
        left = [1] * len(nums)
        leftProduct = 1
        for i in range(1, len(nums)):
            leftProduct *= nums[i-1]
            left[i] = leftProduct
        # e.g. right[3] would be nothing, since there is nothing after the 3rd index
        #   in this example; probably safe to just stick a 1 there
        # right[2] would just be nums[3] = 4
        # right[1] would be nums[2] * nums[3] = 3*4 = 12,
        #   OR, nums[2] * right[2] = 3*4 = 12
        # right[0] would be nums[1] * nums[2] * nums[3] = 2*3*4 = 24,
        #   OR nums[1] * right[1] = 2*12 = 24
        right = [1] * len(nums)
        rightProduct = 1
        for i in range(len(nums)-2, -1, -1):
            rightProduct *= nums[i+1]
            right[i] = rightProduct
        # Now, given that answer[i] is (product of nums[:i]) * (product of nums[i+1:]),
        # we can calculate answer[i] by doing left[i] * right[i].
        # For example, left[0] * right[0] gives us 24, which makes sense, since it's just
        # the product of everything to the right. Sticking a 1 in left[0] before made sense,
        # since 1 does nothing in multiplcation.
        # Meanwhile, e.g. left[2] * right[2] is 2*4 = 8. This also makes sense. answer[2] is
        # the product of nums[0] * nums[1] * nums[3] = 1*2*4 = 8. nums[0] * nums[1] is calculated
        # in left[2] (everything left of 2nd index) and nums[3] is calculated in right[2] (everything
        # right of 2nd index).
        for i in range(len(nums)):
            answer[i] = left[i] * right[i]

        return answer
```

## Initial brute force O(n^2) solution:

```python3
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        answer = [1] * len(nums)
        # This is the brute force O(n^2) solution
        for i in range(len(nums)):
            for j in range(len(nums)):
                if i != j:
                    answer[i] *= nums[j]
        return answer
```
