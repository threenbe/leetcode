# Least Number of Unique Integers after K Removals

Given an array of integers arr and an integer k. Find the least number of unique integers after removing exactly k elements.

https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/


## My solution:

```Java
class Solution {
    public int findLeastNumOfUniqueInts(int[] arr, int k) {
        // key -> value:
        // int element from arr, number of occurrences in arr
        HashMap<Integer, Integer> intOccurrences = new HashMap();
        
        for (int n : arr) {
            int occurrences = intOccurrences.getOrDefault(n, 0);
            intOccurrences.put(n, ++occurrences);
        }
        
        // What we want to do now is sort the keys by frequency in ascending order.
        // This is because deleting the least frequently occurring keys first will
        // result in the least number of unique keys by the end.
        
        // double array, f[i][0] = key, f[i][1] = corresponding value
        int[][] frequencies = new int[intOccurrences.size()][2];
        int freqIndex = 0;
        for (Map.Entry<Integer, Integer> e : intOccurrences.entrySet()) {
            frequencies[freqIndex][0] = e.getKey();
            frequencies[freqIndex++][1] = e.getValue();
        }
        
        Arrays.sort(frequencies, (a,b) -> Integer.compare(a[1],b[1]));
        
        // Now we have an array in which the key,value pairs are ordered by frequency.
        // So, the least number of unique integers after k removals can be solved by
        // effectively removing k elements from the front of the array.
        // e.g.
        // [4, 1, 1, 3, 3, 2], k = 3
        // a resultant double array for this input might look like:
        // [[4,1], [2,1], [1,2], [3,2]]
        // We would want to remove 4, 2, and in this case one of the 1s, which results in
        // two unique integers after the removals (1 and 3). 
        // Note that in this example, we remove a 1 but still have a 1 left over, so we 
        // need to account for the case in which the removal of the last integer doesn't 
        // result in a reduction of the number of unique integers.
        
        int numberOfRemovals = 0;
        int removalIndex = 0;
        while (numberOfRemovals < k) {
            numberOfRemovals += frequencies[removalIndex++][1];
        }
        
        int result = frequencies.length - removalIndex; // the number of unique integers left
        
        // If numberOfRemovals exceeds k, then that means that the last integer we
        // removed still exists in the final array (like in the above example), so
        // account for that here.
        if (numberOfRemovals > k) {
            result++;
        }
        
        return result;
        
    }
        
        
}
```

## My initial solution:
Pretty messy and slow

```Java
class Solution {
    public int findLeastNumOfUniqueInts(int[] arr, int k) {
        // key -> value:
        // int element from arr, number of occurrences in arr
        HashMap<Integer, Integer> intOccurrences = new HashMap();
        
         // key -> value:
        // number of occurrences -> the elements in arr that occur this number of times
        HashMap<Integer, List<Integer>> frequencyMapping = new HashMap();
        int lowestFrequency = Integer.MAX_VALUE;
        
        for (int n : arr) {
            int occurrences = intOccurrences.getOrDefault(n, 0);
            intOccurrences.put(n, ++occurrences);
        }
        
        for (Integer n : intOccurrences.keySet()) {
            int occurrences = intOccurrences.get(n);
            List<Integer> list =
                frequencyMapping.computeIfAbsent(occurrences, p -> new ArrayList<Integer>());
            list.add(n);
            frequencyMapping.put(occurrences, list);
            if (occurrences < lowestFrequency) {
                lowestFrequency = occurrences;
            }
        }
        
        for (int i = 0; i < k; i++) {//remove exactly k elements
            while(!frequencyMapping.containsKey(lowestFrequency)) {
                lowestFrequency++;
            }
            List<Integer> listOfElements = frequencyMapping.get(lowestFrequency);
            int removedElement = listOfElements.remove(0);
            
            //adjust the hashmaps after the removal
            if (listOfElements.isEmpty()){
                frequencyMapping.remove(lowestFrequency);
            }
            if (lowestFrequency > 1) {//the element occurs 1 less time
                lowestFrequency--;
                intOccurrences.put(removedElement, lowestFrequency);
                List<Integer> list =
                    frequencyMapping.computeIfAbsent(lowestFrequency, p -> new ArrayList<Integer>());
                list.add(removedElement);
                frequencyMapping.put(lowestFrequency, list);
            }
            else {//the element is gone completely
                intOccurrences.remove(removedElement);
            }
        }

        return intOccurrences.size();
    }
        
        
}
```
