# First Missing Positive

Given an unsorted integer array nums, return the smallest missing positive integer.

For example, the input [1,0,2] returns 3. The input [3,4,-1,1] returns 2.

You must implement an algorithm that runs in O(n) time and uses constant extra space.

## My solution (with O(n) space...):

```Java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int smallest_pos_num = Integer.MAX_VALUE;
        List<Integer> l = new ArrayList<Integer>();
        for (int i = 0; i < nums.length; i++) {
            int n = nums[i];
            l.add(n);
        }
        Collections.sort(l);
        int res = 0;
        int prev_int = -1;
        for (int i = 0; i < l.size(); i++) {
            int curr_int = l.get(i);
            if (curr_int > 0){
                if (prev_int < 0 && curr_int > 1) {
                    res = 1;
                    break;
                }
                else if (curr_int - prev_int > 1 && prev_int >= 0) {
                    res = prev_int + 1;
                    break;
                }
            }
            prev_int = curr_int;
        }
        if (res > 0) {
            return res;
        }
        else if (prev_int < 0) {
            return 1;
        }
        else {
            return prev_int+1;
        }
    }
}
```
