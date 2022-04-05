# Course Schedule II

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

* For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

## My solution:

```Java
class Solution {
	private Map<Integer, List<Integer>> adjacencyList = new HashMap();

    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // [a,b] means b -> a in a graph
        // construct adjacency list first
        for (int[] edge : prerequisites) {
            int src = edge[1];
            int dst = edge[0];
            List<Integer> list = adjacencyList.getOrDefault(src, new ArrayList());
            list.add(dst);
            adjacencyList.put(src, list);
        }

        // do a topological sort on the graph to get the course order
        // if we detect a cycle at any point, then there is no valid order
        Set<Integer> processing = new HashSet();
        Set<Integer> processed = new HashSet();
        Stack<Integer> stack = new Stack();
        for (int i = 0; i < numCourses; i++) {
            if (topologicalSort(i, processing, processed, stack)) {
                return new int[0];
            }
        }

        int[] courses = new int[stack.size()];
        int i = 0;
        while (!stack.isEmpty()) {
            courses[i++] = stack.pop();
        }

        return courses;
    }

    /* 
     * returns true if it detects a cycle, else returns false
    */
    private boolean topologicalSort(int course, Set<Integer> processing, 
                                    Set<Integer> processed, Stack<Integer> stack) {
	
        if (processed.contains(course)) {
            return false;
        }

        processing.add(course);

        List<Integer> neighbors = adjacencyList.getOrDefault(course, new ArrayList());
        for (Integer neighbor : neighbors) {
            if (processing.contains(neighbor)) {
                return true;
            }
            if (topologicalSort(neighbor, processing, processed, stack)) {
                return true;
            }
        }

        processing.remove(course);
        processed.add(course);
        stack.push(course);
        return false;
    }
    // Time complexity: O(v+e) where v is the number of courses (vertices) and e is the number
    // of edges in the graph. Constructing the adjacency list takes O(e) time, and traversing
    // the graph takes O(v+e) time. 
    // Space complexity: O(v+e). The adjacency list takes up O(e) space to hold the edges. The
    // sets/stack will hold up to O(v) nodes at any given point. 
}
```

## My solution:

```Java
class Solution {
    Map<Integer, List<Integer>> adjacencyList = new HashMap<Integer, List<Integer>>();
    
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        //[a, b] means b -> a (you must take b before a)
        //if it is impossible to finish all courses, then this implies a cyclic graph
        
        for (int n = 0; n < numCourses; n++) {
            adjacencyList.put(n, new ArrayList<Integer>());
        }
        for (int i = 0; i < prerequisites.length; i++) {
            int src = prerequisites[i][1];
            int dest = prerequisites[i][0];
            adjacencyList.get(src).add(dest);
        }
        
        Set<Integer> visiting = new HashSet<Integer>();
        Set<Integer> visited = new HashSet<Integer>();
        Stack<Integer> stack = new Stack<Integer>();
        boolean hasCycle = false;
        for (int n = 0; n < numCourses; n++) {
            hasCycle = topologicalSort(n, visiting, visited, stack);
            if (hasCycle) {
                return new int[0];
            }
        }
        
        int[] courseOrder = new int[stack.size()];
        int index = 0;
        while (!stack.isEmpty()) {
            courseOrder[index++] = stack.pop();
        }
        return courseOrder;
    }
    
    /*topologically sort the classes
     *also return true if a cycle is detected, which indicates that there is no
     *possible topological order of the classes
     */
    private boolean topologicalSort(int courseNum, Set<Integer> visiting, Set<Integer> visited, Stack<Integer> stack) {
        if (visited.contains(courseNum)) {
            return false;
        }
        
        visiting.add(courseNum);
        
        for (Integer neighbor : adjacencyList.get(courseNum)) {
            if (visited.contains(neighbor)) {
                continue;
            }
            if (visiting.contains(neighbor)) {
                return true;
            }
            boolean hasCycle = topologicalSort(neighbor, visiting, visited, stack);
            if (hasCycle) {
                return true;
            }
        }
        
        visiting.remove(courseNum);
        visited.add(courseNum);
        stack.push(courseNum);
        return false;
    }
}
```
