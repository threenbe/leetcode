# Product of the Last K Numbers

Design an algorithm that accepts a stream of integers and retrieves the product of the last k integers of the stream.

Implement the ProductOfNumbers class:

* ProductOfNumbers() Initializes the object with an empty stream.
* void add(int num) Appends the integer num to the stream.
* int getProduct(int k) Returns the product of the last k numbers in the current list. You can assume that always the current list has at least k numbers.

## My solution:

```Java
class ProductOfNumbers {
    List<Integer> list;

    public ProductOfNumbers() {
        list = new ArrayList<Integer>();
    }
    
    public void add(int num) {
        if (num == 0) {
            list = new ArrayList<Integer>();
        }
        else if (list.isEmpty()) {
            list.add(num);
        }
        else {
            list.add(num * list.get(list.size()-1));
        }
    }
    
    public int getProduct(int k) {
        int div;
        int size = list.size();
        if (k > size) {
            return 0;
        }
        else if (k == size) {
            div = 1;
        }
        else {
            div = list.get(size-1-k);
        }
        return list.get(size-1)/div;
    }
}

/**
 * Your ProductOfNumbers object will be instantiated and called as such:
 * ProductOfNumbers obj = new ProductOfNumbers();
 * obj.add(num);
 * int param_2 = obj.getProduct(k);
 */
```
