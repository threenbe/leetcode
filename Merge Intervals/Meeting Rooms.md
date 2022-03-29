# Meeting Rooms

Given an array of meeting time intervals where intervals[i] = [starti, endi], determine if a person could attend all meetings.

https://leetcode.com/problems/meeting-rooms/

## My solution:

```Java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        // Basically, if any of the intervals overlap, then you can't attend
        // all the meetings.
        Arrays.sort(intervals, (a,b) -> Integer.compare(a[0],b[0]));
        for (int i = 0; i < intervals.length-1; i++) {
            if (intervals[i][1] > intervals[i+1][0]) {
                return false;
            }
        }
        return true;
    }
    
    // Time complexity: O(n*log(n)) due to sorting
    // Space complexity: O(1)
}
```
