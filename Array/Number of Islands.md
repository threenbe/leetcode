# Number of Islands

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

## My solution:

```Java
class Solution {
    public int numIslands(char[][] grid) {
        //we can treat the grid as an adjacency matrix
        //start at a node marked 1 and then traverse the graph until done, then update a count
        //while traversing the graph, mark each node by setting it to 0 in the graph
        //subsequent traversals will only be carried out from nodes marked as 1
        
        int numRows = grid.length;
        int numCols = grid[0].length;
        int numOfIslands = 0;
        for (int r = 0; r < numRows; r++) {
            for (int c = 0; c < numCols; c++) {
                if (grid[r][c] == '1') {
                    numOfIslands++;
                    traverse(grid, r, c);
                }
            }
        }
        return numOfIslands;
    }
    
    private void traverse(char[][] grid, int r, int c) {
        int numRows = grid.length;
        int numCols = grid[0].length;
        
        if (r < 0 || c < 0 || r >= numRows || c >= numCols || grid[r][c] == '0') {
            return;
        }
        
        grid[r][c] = '0';
        
        traverse(grid, r-1, c);
        traverse(grid, r+1, c);
        traverse(grid, r, c-1);
        traverse(grid, r, c+1);
    }
}
```
