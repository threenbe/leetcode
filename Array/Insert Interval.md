# Insert Interval

You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

https://leetcode.com/problems/insert-interval/

## My solution:

```Java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        LinkedList<int[]> list = new LinkedList<>();
        //First, add all the intervals that start before newInterval
        int idx;
        for (idx = 0; idx < intervals.length && intervals[idx][0] < newInterval[0]; idx++) {
            list.add(intervals[idx]);
        }
        
        // Now, add newInterval, and merge as needed
        if (!list.isEmpty() && list.getLast()[1] >= newInterval[0]) {
            list.getLast()[1] = Math.max(list.getLast()[1], newInterval[1]);
        }
        else {
            list.add(newInterval);
        }
        
        // Now, add the remaining intervals, and merge as needed
        for (; idx < intervals.length; idx++) {
            if (list.getLast()[1] >= intervals[idx][0]) {
                list.getLast()[1] = Math.max(list.getLast()[1], intervals[idx][1]);
            }
            else {
                list.add(intervals[idx]);
            }
        }
        
        return list.toArray(new int[list.size()][2]);
    }
    
    // Time complexity: O(n)
    // Space complexity: O(n) to hold the output
}
```
