# Sliding Window Median

The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle values.

* For examples, if arr = [2,3,4], the median is 3.
* For examples, if arr = [1,2,3,4], the median is (2 + 3) / 2 = 2.5.

You are given an integer array nums and an integer k. There is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the median array for each window in the original array.

https://leetcode.com/problems/sliding-window-median/

## My solution:

```Java
class Solution {
    private PriorityQueue<Integer> lower = new PriorityQueue<Integer>(Collections.reverseOrder());//max heap
    private PriorityQueue<Integer> higher = new PriorityQueue<Integer>();//min heap
    
    public double[] medianSlidingWindow(int[] nums, int k) {
        double[] result = new double[nums.length-k+1];
        
        for (int i = 0; i < k; i++) {
            addToHeaps(nums[i]);
        }
        
        for (int i = 0; i < result.length; i++) {
            result[i] = getMedian();
            removeFromHeaps(nums[i]);
            if (i < result.length-1) {
                addToHeaps(nums[i+k]);
            }
        }
        return result;
    }
    
    private void addToHeaps(int num) {
        lower.add(num);
        higher.add(lower.poll());
        if (lower.size() < higher.size()) {
            lower.add(higher.poll());
        }
    }
    
    private void removeFromHeaps(int num) {
        if (lower.contains(num)) {
            lower.remove(num);
            if (lower.size() < higher.size()) {
                lower.add(higher.poll());
            }
        }
        else {//higher contains num
            higher.remove(num);
            if (lower.size() - higher.size() == 2) {
                higher.add(lower.poll());
            }
        }
    }
    
    private double getMedian(){
        return lower.size() == higher.size() ?
            ((double)lower.peek()+(double)higher.peek())/2 : (double)lower.peek();
    }
```
