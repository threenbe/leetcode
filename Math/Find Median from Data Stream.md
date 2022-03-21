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


## My initial solution:

This is actually what I came up with first, although I think the solution with heaps is both superior and also way easier to remember and implement.

The idea I had was that we use binary search to figure out where to insert the next element we receive and keep the ArrayList sorted at all times, which is a O(log(n)) operation. 

The problem is that inserting an element at a given index in an ArrayList is still an O(n) operation, whereas insertion and deletion from a heap is O(log(n)). So this solution should be O(n) in time complexity overall, whereas the one above should be O(log(n)).

```Java
class MedianFinder {
    
    ArrayList<Integer> al;

    public MedianFinder() {
        al = new ArrayList<Integer>();
    }
    
    public void addNum(int num) {
        if (al.isEmpty()) {
            al.add(num);
        }
        else {
            int idxToInsert = Collections.binarySearch(al, num);
            if (idxToInsert < 0) {
                idxToInsert = -1*(idxToInsert+1);
            }
            al.add(idxToInsert, num);
        }
    }
    
    public double findMedian() {
        int size = al.size();
        int idx = size/2;
        if ((size % 2) == 1) {
            return (double) al.get(idx);
        }
        else {
            double a = al.get(idx);
            double b = al.get(idx-1);
            return (a+b)/2;
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
