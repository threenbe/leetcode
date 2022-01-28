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
