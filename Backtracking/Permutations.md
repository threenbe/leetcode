# Permutations

Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

# My solution:

```Java
class Solution {
    public void generatePermutations(int permutation_len, List<List<Integer>>output, 
                                      List<Integer> num_list, int first_index) {
        // This uses the backtracking algorithm to generate each permutation.
        // Say we start with [1,2,3].
        // Start by swapping the first number with all others, i.e. swap to get
        // [2,1,3] and [3,2,1].
        // Then from those three lists, swap the second number with all the numbers after
        // that, and so on (so in this case, just swap the last two numbers).
        // So, with our very first starting point, we'd swap to get [1,3,2], and so on.
        if (first_index == permutation_len) {
            output.add(new ArrayList<Integer>(num_list));
        }
        for (int i = first_index; i < permutation_len; i++) {
            Collections.swap(num_list, first_index, i);
            generatePermutations(permutation_len, output, num_list, first_index+1);
            //backtrack
            Collections.swap(num_list, first_index, i);
        }
        /* So let's walk through the logic above with our example.
         * 1. In the first iteration of our loop, we swap indices 0 and 0, so the list is unchanged, [1,2,3].
         * 2. We then recursively call backtrack and swap indices 1 and 1, so it's unchanged again.
         * 3. We recurse again and swap indices 2 and 2, so it's unchanged again.
         * 4. Finally we recuse once more and hit the if statement, so we add [1,2,3] to the output list and return.
         * 5. We return to step 3, after which the for loop terminates and returns.
         * 6. We return to step 2, where our current num_list is [1,2,3] and our indices are 1,1. 
         *    We increment i and then swap indices 1 and 2 to get [1,3,2].
         * 7. We then recurse and swap indices 2 and 2, and then recurse again and hit the if statement. 
         *    [1,3,2] is added to the output list, so we return.
         * 8. We return to step 6, where we backtrack and change our array back to [1,2,3]. The for loop 
         *    terminates on the next iteration so we return.
         * 9. We finally return to step 1. We backtrack on our 0,0 swap (no change) and increment i. 
         *    We then swap indices 0 and 1 to get [2,1,3].
         * 10. We recurse and then swap 1,1, and then recurse again to get 2,2, and then recurse again to 
         * add [2,1,3] to the output list. I think at this point the idea is clear.
        */
    }
    
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> output = new ArrayList<>();
        List<Integer> nums_list = new ArrayList<>();//this will get added to the output
        for (int n : nums)
            nums_list.add(n);
        int permutation_len = nums_list.size();
        generate_permutations(permutation_len, output, nums_list, 0);
        return output;
    }
}
```
