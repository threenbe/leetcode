# Merge Intervals

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

## My solution:

```Java
class Solution {
    public int[][] merge(int[][] intervals) {
        //Sort each array by their first element
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        LinkedList<int[]> mergedList = new LinkedList<int[]>();
        for(int[] interval : intervals) {
            if (mergedList.isEmpty() || mergedList.getLast()[1] < interval[0]) {
                //the interval does not overlap with the previous interval
                mergedList.add(interval);
            }
            else {
                mergedList.getLast()[1] = Math.max(interval[1], mergedList.getLast()[1]);
            }
        }
        return mergedList.toArray(new int[mergedList.size()][]);
    }
}
```
