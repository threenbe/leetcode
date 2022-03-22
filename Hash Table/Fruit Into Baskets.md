# Fruit Into Baskets

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array fruits where fruits[i] is the type of fruit the ith tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

* You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
* Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
* Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array fruits, return the maximum number of fruits you can pick.

## My solution:

```Java
class Solution {
    public int totalFruit(int[] fruits) {
        // Put simply:
        // Find the subarray in which you can pick the most fruits,
        // with the restriction that you may only pick two types of fruits.
        
        // key, val -> fruit type, the number of fruits picked of that type
        Map<Integer, Integer> fruitFreq = new HashMap<>();
        int left = 0;
        int right = 0;
        int currentFruitCount = 0;
        int maxFruits = 0;
        
        while (right < fruits.length) {
            int currentFruit = fruits[right++];
            fruitFreq.put(currentFruit, fruitFreq.getOrDefault(currentFruit, 0) + 1);
            currentFruitCount++;
            
            while (fruitFreq.size() > 2) {
                int leftmostFruit = fruits[left++];
                int updatedOccurrences = fruitFreq.get(leftmostFruit) - 1;
                currentFruitCount--;
                if (updatedOccurrences == 0) {
                    fruitFreq.remove(leftmostFruit);
                }
                else {
                    fruitFreq.put(leftmostFruit, updatedOccurrences);
                }
            }
            
            maxFruits = Math.max(currentFruitCount, maxFruits);
        }
        
        return maxFruits;
    }
}
```
