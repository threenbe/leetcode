# Two Sum

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

## My solution:

```Java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] sol = {-1,-1};
        for (int i = 0; i < nums.length; i++){
            int second = target - nums[i];
            for (int j = i+1; j < nums.length; j++){
                if (nums[j] == second){
                    sol[0] = i;
                    sol[1] = j;
                    break;
                } 
            }
            if (sol[0] != -1) break;
        }
        return sol;
    }
}
```

## My solution that uses a HashMap:

```Java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int solution[] = new int[]{-1,-1};
        HashMap<Integer, Integer> sumMap = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++) {
            sumMap.put(target-nums[i], i);
        }
        for (int i = 0; i < nums.length; i++) {
            if (sumMap.containsKey(nums[i]) && sumMap.get(nums[i]) != i) {
                solution[0] = sumMap.get(nums[i]);
                solution[1] = i;
                return solution;
            }
        }
        return null;
    }
}
```
