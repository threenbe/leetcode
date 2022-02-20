# Course Schedule II

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

* For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

## My solution

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
