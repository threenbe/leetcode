# Meeting Rooms II

Given an array of meeting time intervals intervals where intervals[i] = [starti, endi], return the minimum number of conference rooms required.

https://leetcode.com/problems/meeting-rooms-ii/

## My solution:

```Java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        // The minimum number of rooms required is equal to the largest
        // number of meetings that overlap all at once.
        // This is a problem that involves scheduling tasks, so I feel like it makes
        // sense to use a priority queue.
        
        Arrays.sort(intervals, (a,b) -> Integer.compare(a[0],b[0]));
        
        // We store each meeting's end time in this priority queue.
        // When we're adding a new meeting to the queue, we check to
        // see if any of the current meetings have ended or are about
        // to end. If so, we can remove that meeting and then add the
        // new one (this simulates giving the new meeting an old room).
        // If not, then we just add the new meeting (this simulates giving
        // the new meeting a new room).
        PriorityQueue<Integer> rooms = new PriorityQueue<>();
        rooms.add(intervals[0][1]);
        //int minRooms = 1;
        
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] >= rooms.peek()) {
                rooms.poll();
            }
            rooms.add(intervals[i][1]);
            //minRooms = Math.max(rooms.size(), minRooms);
        }
        
        //return minRooms;
        return rooms.size();
        
        // I thought it'd be necessary to keep track of the highest number of rooms
        // occupied at any given point, but that isn't necessary. Since we only evict
        // people out of a room on an as-needed basis, which is when the meeting we're
        // currently processing does NOT overlap one of the meetings in the priority queue,
        // the size of the priority queue by the end will inevitably correspond to the highest
        // number of meetings that overlap at once, which is the number of rooms we want.
        
        // Take the following examples:
        
        // [[0,30],[5,10],[15,20],[40,50]] -> needs 2 rooms
        // pq = [30] -> pq = [10,30] -> evict 10, pq = [20,30] -> evict 20, pq = [30,40], size 2
        // Looking at the initial input, you might think that final pq size might be 1 because
        // the last meeting doesn't overlap any of the others, but it won't be; we only need
        // to clear out one of two occupied rooms for the last meeting in our algorithm.
        
        // [[0,30],[5,10],[15,20],[12,25]] -> needs 3 rooms
        // pq = [30] -> pq = [10,30] -> evict 10, pq = [20,30] -> pq = [20,25,30], size 3
        // Let's expand the input to [[0,30],[5,10],[15,20],[12,25],[60,90],[80,100]], 
        // still needs only 3 rooms
        // pq = [20,25,30] -> evict 20, pq = [25,30,60] -> evict 20, [30,60,80]
    }
    
    // Time complexity: O(n*log(n)) due to the array sort. Also, in the worst case where all
    // meetings collide, we have to enqueue/dequeue n elements, which is a log(n) operation
    // for each one.
    // Space complexity: O(n) for the priority queue.
}
```
