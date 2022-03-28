# Merge Intervals

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

https://leetcode.com/problems/merge-intervals/

## My solution:

```Java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a,b) -> Integer.compare(a[0],b[0]));
        LinkedList<int[]> list = new LinkedList<>();
        
        for (int[] interval : intervals) {
            if (!list.isEmpty() && list.getLast()[1] >= interval[0]) {
                list.getLast()[1] = Math.max(list.getLast()[1], interval[1]);
            }
            else {
                list.add(interval);
            }
        }
        
        return list.toArray(new int[list.size()][2]);
    }
    
    // Time complexity: O(n*log(n)) due to array sort
}
```
