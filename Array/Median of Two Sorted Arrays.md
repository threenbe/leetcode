# Median of Two Sorted Arrays

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).

## My (O(m+n)...) solution:

```Java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        double[] mergedArray = merge(nums1, nums2);
        int medianIndex = (nums1.length+nums2.length)/2;
        if ((nums1.length+nums2.length) % 2 == 1) {
            return mergedArray[medianIndex];
        }
        else {
            return (mergedArray[medianIndex-1]+mergedArray[medianIndex])/2;
        }
    }

    private double[] merge(int[] arr1, int[] arr2) {
        double[] mergedArray = new double[arr1.length+arr2.length];
        int i = 0, j = 0, k = 0;
        while(i < arr1.length && j < arr2.length) {
            if (arr1[i] < arr2[j]) {
                mergedArray[k++] = arr1[i++];
            }
            else {
                mergedArray[k++] = arr2[j++];
            }
        }
        while (i < arr1.length) {
            mergedArray[k++] = arr1[i++];
        }
        while(j < arr2.length) {
            mergedArray[k++] = arr2[j++];
        }
        return mergedArray;
    }
}
```
