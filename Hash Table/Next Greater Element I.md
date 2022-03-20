# Next Greater Element I

The problem description on leetcode is complete garbage. Basically, you're given two arrays, nums1 and nums2. nums1 is a subset of nums2. For each element in nums1, find the same element in nums2 and then find an element to the right of it that's greater than it. Store the greater element in an output array. If there is no greater element to its right, store -1.

## My solution:

```Java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] result = new int[nums1.length];
        HashMap<Integer, Integer> map = new HashMap<>();//element, index
        for (int i = 0; i < nums2.length; i++) {
            map.put(nums2[i], i);
        }
        //find next greater element of each elem in nums1
        for (int i = 0; i < nums1.length; i++) {
            Integer indexInNums2 = map.get(nums1[i]);
            result[i] = -1;
            for (int j = indexInNums2+1; j < nums2.length; j++) {
                if (nums2[j] > nums2[indexInNums2]) {
                    result[i] = nums2[j];
                    break;
                }
            }
        }
        return result;
    }
}
```
