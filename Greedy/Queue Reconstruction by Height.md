# Queue Reconstruction by Height

You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.

Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).

https://leetcode.com/problems/queue-reconstruction-by-height/

## My solution:

```Java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, (p1,p2) -> p1[0] == p2[0] ? p1[1]-p2[1] : p2[0]-p1[0]);
        LinkedList<int[]> result = new LinkedList<>();
        
        for (int[] p : people) {
            result.add(p[1], p); // use k value as index to insert at
        }
        
        return result.toArray(new int[people.length][2]);
        
        /* 
         * (h,k) -> person of height h with k people in front who are of height >= h
         * The tallest people will always have no one in front of them, or only people
         * who are of the same height in front of them.
         * Example:
         * people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
         * First, sort by height, and for those of equal height, put the ones with more
         * people in front of them after the others:
         * [[7,0],[7,1],[6,1],[5,0],[5,2],[4,4]]
         * Now we iterate over the array, starting from the tallest, and sort them accordingly:
         * [7,0] is the tallest with no one in front of him:
         *  [[7,0]]
         * [7,1] is also the tallest, but has one person in front of him; insert at idx 1:
         *  [[7,0],[7,1]]
         * [6,1] is the next tallest, and has one person taller than him, so he falls between
         * the two tallest. Insert at idx 1:
         *  [[7,0],[6,1],[7,1]]
         * [5,0] has no one taller or equal in front of him, so he must be in front of all the
         * rest; insert at idx 0:
         *  [[5,0],[7,0],[6,1],[7,1]]
         * [5,2] has two people taller/equal in front of him, insert at idx 2:
         *  [[5,0],[7,0],[5,2],[6,1],[7,1]]
         * Lastly, [4,4] has 4 people taller/equal in front, so insert at idx 4:
         *  [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
         * This wasn't immediately obvious to me at first, but it makes sense: after the initial
         * sorting, you can just sort by using each person's k value as an index when
         * reconstructing the result array. This is because we construct the array with the
         * tallest people first, so when adding a new person, every person in the current result
         * array is either of equal height or taller. Therefore, the number of people in front
         * of him that are of equal/greater height also represents the index at which he can
         * be placed.
         */
    }
}
```
