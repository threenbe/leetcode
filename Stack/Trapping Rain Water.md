# Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

https://leetcode.com/problems/trapping-rain-water/

## My solution:

```Java
class Solution {
    public int trap(int[] height) {
        Stack<Integer> elevations = new Stack<Integer>();
        int result = 0;
        for (int i = 0; i < height.length; i++) {
            while(!elevations.isEmpty() && height[i] > height[elevations.peek()]) {
                int stackTop = elevations.pop();
                /* height[i] is the elevation to the right of the water
                 * height[elevations.peek()] i.e. the current top after the pop is the elevation to its left */
                
                if (elevations.isEmpty()) break;

                /* There is water at any elevation that is bounded by two taller elevations.
                 * The amount of water at said elevation is dependent on the lower of the 
                 * two taller elevations. */
                int distance = i - elevations.peek() - 1;//number of blocks bounded by the two tall elevations
                int water = Math.min(height[i], height[elevations.peek()]) - height[stackTop];
                result += distance*water;
            }
            elevations.push(i);
        }
        return result;
    }
}
```
