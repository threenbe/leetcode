# Container With Most Water

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

**Notice** that you may not slant the container.

## My solution:

```Java
class Solution {
    public int maxArea(int[] height) {
        // There are two things that can increase the amount of water that
        // our potential container can hold -- one is the width between the
        // container's endpoints, and the other is the height of the shortest
        // endpoint.
        // So, it makes sense to start at the two endpoints furthest away from each other,
        // as this gives us the most width. Then, the only way we could possibly hold more
        // water is if one of the inner lines is taller than our current shortest endpoint
        // by enough to make up for the shorter width.
        int leftLine = 0;
        int rightLine = height.length - 1;
        int maxArea = Math.min(height[leftLine], height[rightLine]) * (rightLine - leftLine);
        while (leftLine < rightLine) {
            if (height[leftLine] < height[rightLine]) {
                leftLine++;
            }
            else {
                rightLine--;
            }
            maxArea = Math.max(
                    Math.min(height[leftLine], height[rightLine]) * (rightLine - leftLine),
                    maxArea);
        }
        return maxArea;
    }
}
```
