# Interval List Intersections

You are given two lists of closed intervals, firstList and secondList, where firstList[i] = [starti, endi] and secondList[j] = [startj, endj]. Each list of intervals is pairwise disjoint and in sorted order.

Return the intersection of these two interval lists.

A closed interval [a, b] (with a <= b) denotes the set of real numbers x with a <= x <= b.

The intersection of two closed intervals is a set of real numbers that are either empty or represented as a closed interval. For example, the intersection of [1, 3] and [2, 4] is [2, 3].

https://leetcode.com/problems/interval-list-intersections/

## My solution:

```Java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        int firstLength = firstList.length;
        int secondLength = secondList.length;
        int firstIdx = 0;
        int secondIdx = 0;
        
        LinkedList<int[]> output = new LinkedList<>();
        
        while (firstIdx < firstLength && secondIdx < secondLength) {
            // firstInterval should always be the one that starts sooner
            int[] firstInterval = firstList[firstIdx][0] < secondList[secondIdx][0] ?
                firstList[firstIdx] : secondList[secondIdx];
            int[] secondInterval = firstList[firstIdx][0] < secondList[secondIdx][0] ?
                secondList[secondIdx] : firstList[firstIdx];
            
            // Check to see if they have overlap. If so, then take the interval of the overlap 
            // and add it to the list. Keep the interval with a larger end point since that
            // one might intersect with the next interval in the other list.
            // If they don't overlap, don't add anything. Again, discard the interval with the
            // smaller end point and keep the one that might overlap with the next interval.
            
            if (firstInterval[1] >= secondInterval[0]) {//overlap
                int[] intersection = new int[]{secondInterval[0], Math.min(firstInterval[1], secondInterval[1])};
                output.add(intersection);
            }
            
            if (firstList[firstIdx][1] > secondList[secondIdx][1]) {
                secondIdx++;
            }
            else {
                firstIdx++;
            }
        }
        
        return output.toArray(new int[output.size()][2]);
    }
    
    // Example:
    // [[0,2],[5,10],[13,23],[24,25]], [[1,5],[8,12],[15,24],[25,26]]
    // firstInterval = [0,2], secondInterval = [1,5] -> newInterval = [1,2]
    // [[5,10],[13,23],[24,25]], [[1,5],[8,12],[15,24],[25,26]], output: [[1,2]]
    // firstInterval = [1,5], secondInterval = [5,10] -> newInterval[5,5]
    // [[5,10],[13,23],[24,25]], [[8,12],[15,24],[25,26]], output: [[1,2],[5,5]]
    // firstInterval = [5,10], secondInterval = [8,12] -> newInterval [8, 10]
    // [[13,23],[24,25]], [[8,12],[15,24],[25,26]], output: [[1,2],[5,5],[8,10]]
    // firstInterval = [8,12], secondInterval = [13,23] -> newInterval = none
    // [[13,23],[24,25]], [[15,24],[25,26]], output [[1,2],[5,5],[8,10]]
    // firstInterval = [13,23], secondInterval = [15,24] -> newInterval = [15,23]
    // [[24,25]], [[15,24],[25,26]], output: [[1,2],[5,5],[8,10],[15,23]]
    // firstInterval = [15,24], secondInterval = [24,25] -> newInterval = [24,24]
    // [[24,25]], [[25,26]], output: [[1,2],[5,5],[8,10],[15,23],[24,24]]
    // firstInterval = [24,25], secondInterval = [25,26] -> newInterval = [25,25]
    // [], [[25,26]], output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
    // loop terminates
    
    // Time complexity: O(n) where n is the number of elements in both lists
    // Space complexity: O(n) for the space used to hold the output
}
```
