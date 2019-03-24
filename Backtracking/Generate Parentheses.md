# Generate Parentheses 

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

## My solution:

```Java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ret = new ArrayList<String>();
        generateParenthesis(ret, "", 0, 0, n);
        return ret;
        
        /*WRONG SOLN, failed to account for cases such as n=4 -> (())(()) where it's two n=2 combos combined
        //you can probably recursively do combos of 1, then append combos of 2, etc.
        //so for n=1 you'll only have ()
        //for n=2 you'll have ()() and (())
        //then n=3 will be various combinations of the above, e.g. (())(), 
        //plus a new one like ((())) which is taking (()) and putting a new () around it
        if (n == 0) return null;
        else if (n == 1) {
            return Arrays.asList("()");
        }
        else {
            List<String> l1 = generateParenthesis(n-1);
            List<String> l2 = new ArrayList<String>();
            //we'll want to form ()() and (())
            //you can append () to the end, to the start, or all around
            //need to make sure there aren't repeats when appending
            for (int i = 0; i < l1.size(); i++) {
                String s = l1.get(i);
                String s1 = s + "()";
                String s2 = "()" + s;
                l2.add(s1);
                if (!s2.equals(s1)) {
                    l2.add(s2);
                }
                String s3 = "(" + s + ")";
                l2.add(s3);
            }
            return l2;
        }*/
    }
    
    private void generateParenthesis(List<String> l, String combo, int open, int close, int n) {
        if (combo.length() == n*2) {
            l.add(combo);
            return;
        }
        
        if (open < n) {
            generateParenthesis(l, combo+"(", open+1, close, n);
        }
        if (close < open) {
            generateParenthesis(l, combo+")", open, close+1, n);
        }
    }
}
```
