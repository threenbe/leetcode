# Two Sum III - Data structure design

Design a data structure that accepts a stream of integers and checks if it has a pair of integers that sum up to a particular value.

Implement the TwoSum class:

* TwoSum() Initializes the TwoSum object, with an empty array initially.
* void add(int number) Adds number to the data structure.
* boolean find(int value) Returns true if there exists any pair of numbers whose sum is equal to value, otherwise, it returns false.

## My solution:

```Java
class TwoSum {
    // key, val -> number, the number of times it occurs in this data stream
    private HashMap<Integer, Integer> valueFreq;

    public TwoSum() {
        valueFreq = new HashMap<Integer, Integer>();
    }
    
    public void add(int number) {
        valueFreq.put(number, valueFreq.getOrDefault(number, 0) + 1);
    }
    
    public boolean find(int value) {
        for (Integer key : valueFreq.keySet()) {
            int complement = value - key;
            if (key != complement) {
                if (valueFreq.containsKey(complement)) {
                    return true;
                }
            }
            else if (valueFreq.get(key) > 1) {
                return true;
            }
            
        }
        return false;
    }
}

/**
 * Your TwoSum object will be instantiated and called as such:
 * TwoSum obj = new TwoSum();
 * obj.add(number);
 * boolean param_2 = obj.find(value);
 */
```
