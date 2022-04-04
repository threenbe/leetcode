# Course Schedule

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

* For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return true if you can finish all courses. Otherwise, return false.

https://leetcode.com/problems/course-schedule/

## My solution:

```Java
class Solution {
    // key, val -> course, list of courses for which the key 'course' is a prereq for
    private Map<Integer, List<Integer>> adjacencyList = new HashMap<>();
    
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // [a, b] means that b -> a in a graph (b must be taken before a)
        // This problem is basically just asking if there is a cycle in the graph.
        
        // construct adjacency list
        for (int i = 0; i < numCourses; i++) {
            adjacencyList.put(i, new ArrayList<Integer>());
        }
        for (int[] edge : prerequisites) {
            int src = edge[1];
            int dst = edge[0];
            adjacencyList.get(src).add(dst);
        }
        
        // find cycle, return true if found, else false
        Set<Integer> processing = new HashSet<>();
        Set<Integer> processed = new HashSet<>();
        for (int i = 0; i < numCourses; i++) {
            if (hasCycle(i, processing, processed)) {
                // cycle exists, so we can't finish
                return false;
            }
        }
        // no cycle exists, so we can finish
        return true;
    }
    
    private boolean hasCycle(int course, Set<Integer> processing, Set<Integer> processed) {
        // check to see if we've already processed the current node and its neighbors
        if (processed.contains(course))
            return false;
        
        // currently processing this node
        processing.add(course);
        
        List<Integer> neighbors = adjacencyList.get(course);
        for (Integer neighbor : neighbors) {
            // if we encounter a neighbor that hasn't finished processing yet,
            // then that means there exists a path back to it from one of its
            // children, i.e. a cycle
            if (processing.contains(neighbor)) {
                return true;
            }
            else if (hasCycle(neighbor, processing, processed)) {
                return true;
            }
        }
        
        // finished processing the current node and its children
        processing.remove(course);
        processed.add(course);
        return false;
    }
    
    // Time complexity: O(e + v), where v is the number of courses (vertices) and e is the 
    // number of dependencies (edges). It takes O(e) time to construct the adjacency list, 
    // and it takes O(v) time to visit each node in the graph.
    // Space complexity: O(e + v). Our adjacency list holds v nodes and e edges, and our
    // processing/processed sets will grow to hold up to v nodes at a time.
}
```
