# Find Median from Data Stream

The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

* For example, for arr = [2,3,4], the median is 3.
* For example, for arr = [2,3], the median is (2 + 3) / 2 = 2.5.

Implement the MedianFinder class:

* MedianFinder() initializes the MedianFinder object.
* void addNum(int num) adds the integer num from the data stream to the data structure.
* double findMedian() returns the median of all elements so far. Answers within 10-5 of the actual answer will be accepted.

## My solution:

```Java
class MedianFinder {
    PriorityQueue<Integer> minHeap;
    PriorityQueue<Integer> maxHeap;

    public MedianFinder() {
        minHeap = new PriorityQueue<Integer>();
        maxHeap = new PriorityQueue<Integer>(Collections.reverseOrder());
    }
    
    public void addNum(int num) {
        // The max heap will contain all elements less than or equal to the median.
        // The min heap will contain all elements greater than the median.
        // Therefore, all elements in the max heap will be smaller than those in the min heap.
        // The max heap's size will always be equal to the min heap's or no greater
        // than the min heap's by 1. That way, the median can be found by either peeking
        // the max heap or averaging the peeks of both heaps.
        
        maxHeap.add(num);//just add the num here first
        //this will re-sort the data such that the median is right at the tops of the heaps
        minHeap.add(maxHeap.poll());
        
        //maxHeap's size should always be equal to or greater than minHeap by 1 for this 
        //solution, for consistency in retrieving the median later. The previous operation
        //could result in the median ending up in the minHeap, so if we break our size
        //property, fix it here.
        if (maxHeap.size() < minHeap.size()) {
            maxHeap.add(minHeap.poll());
        }

        // e.g. [2,4,3]
        // add 2
        // maxHeap(2) minHeap()
        // add 4
        // maxHeap(2,4) -> maxHeap(2) minHeap(4)
        // add 3
        // maxHeap(2,3) minHeap(4) -> maxHeap(2) minHeap(3,4) -> maxHeap(2,3) minHeap(4)
        // median is 3
        // if we were to add some other number (e.g. 5, which would go into the min heap) then
        // the median would be the tops of each heap averaged out
    }
    
    public double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return (double) maxHeap.peek();
        }
        else {//equal size
            return ((double)maxHeap.peek()+(double)minHeap.peek())/2;
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```
