# Sliding Window Median

The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle values.

* For examples, if arr = [2,3,4], the median is 3.
* For examples, if arr = [1,2,3,4], the median is (2 + 3) / 2 = 2.5.

You are given an integer array nums and an integer k. There is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the median array for each window in the original array.

https://leetcode.com/problems/sliding-window-median/

## My solution:

I initially implemented this problem using Priority Queues (see below), but it turns out that that's not the best way to do it in Java.

The PriorityQueue's remove(Object o) method has O(n) time complexity, because the PriorityQueue doesn't guarantee that its elements are sorted, aside from the root being the least or greatest element. Because of this, the overall complexity of the solution ends up being O(n\*k).

TreeSet guarantees that its elements are sorted, so its remove(Object o) method has O(log(n)) time complexity (incidentally, this also means that I didn't really have to provide the two TreeSets with different Comparators, but whatever, it's more intuitive that way in my head). The problem with TreeSets is that they don't allow for duplicate elements, but we can get around that by storing the indices of each element, which will always be unique. With this approach, the overall time complexity of the solution should be O(n\*log(k)).

```Java
class Solution {
    private TreeSet<Integer> lower;
    private TreeSet<Integer> higher;
    
    public double[] medianSlidingWindow(int[] nums, int k) {
        //TreeSets don't allow for duplicates, but we can get around that by storing indices
        Comparator<Integer> comparator = 
            (a,b) -> nums[a] != nums[b] ? 
            Integer.compare(nums[a],nums[b]) : Integer.compare(b,a);
        lower = new TreeSet<Integer>(comparator.reversed());//max treeset
        higher = new TreeSet<Integer>(comparator);//min treeset

        double[] result = new double[nums.length-k+1];
        
        for (int i = 0; i < k; i++) {
            addToSets(i);
        }
        
        for (int i = 0; i < result.length; i++) {
            result[i] = getMedian(nums, k);
            removeFromSets(i);
            if (i < result.length-1) {
                addToSets(i+k);
            }
        }
        return result;
    }
    
    private void addToSets(int num) {
        lower.add(num);
        higher.add(lower.pollFirst());
        if (lower.size() < higher.size()) {
            lower.add(higher.pollFirst());
        }
    }
    
    //I had some overly complicated logic for removing stuff from the sets before, but it
    //isn't actually necessary. 
    //Suppose lower.size() > higher.size(). My concern before was that the element we were
    //removing might be in "higher" and not "lower," and that removing it from "higher" would 
    //result in lower.size() being greater than higher.size() by 2, which breaks our size 
    //property.
    //But this isn't a real problem here. Consider if lower.size() is 4 and higher.size() is 2
    //after a removal. We then call addToSets() and add an element to lower, increasing its
    //size to 5. Then, we remove the max element in lower and add it to higher, which results
    //in lower.size() == 4 and higher.size() == 3. We've restored our size property.
    //Likewise, if lower.size() and higher.size() are both 4, and then we remove an element
    //from lower (which breaks our size property), it's fine. addToSets will add an element
    //to lower (lower.size() == 4) and then move lower's max to higher (lower.size() == 3,
    //higher.size() == 5). We will then see that lower.size() is less than higher.size() and
    //move higher's min to lower (lower.size() == 4, higher.size() == 4). Again, the size
    //property is restored.
    private void removeFromSets(int num) {
        lower.remove(num);
        higher.remove(num);
        /*if (lower.contains(num)) {
            lower.remove(num);
            if (lower.size() < higher.size()) {
                lower.add(higher.pollFirst());
            }
        }
        else {//higher contains num
            higher.remove(num);
            if (lower.size() - higher.size() == 2) {
                higher.add(lower.pollFirst());
            }
        }*/
    }
    
    private double getMedian(int[] nums, int k){
        return k % 2 == 0 ?
            ((double)nums[lower.first()]+(double)nums[higher.first()])/2.0 
            : (double)nums[lower.first()];
    }
}
```

## My initial solution:

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
