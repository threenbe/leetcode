# Least Number of Unique Integers after K Removals

Given an array of integers arr and an integer k. Find the least number of unique integers after removing exactly k elements.

https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/

# My initial solution:
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
