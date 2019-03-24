# Palindrome Number

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

## My solution:

```Java
class Solution {
    public boolean isPalindrome(int x) {
        String num = Integer.toString(x);
        //if even chars, first half = second half backwards
        //if odd chars, first half = second half backwards excluding middle char
        //e.g. for len=7, check idx 0-2 and 4-6
        //e.g. for len=6, check idx 0-2 and 3-5
        int size = num.length();
        if (size == 1) return true;
        
        int half_size = size/2;
        int second_half = half_size+(size%2);
        
        //int i = 0, j = size-1;
        for (int i = 0, j = size-1; i < half_size && j >= second_half; i++, j--){
            if (num.charAt(i) != num.charAt(j)) return false;
        }
        return true;
    }
}
```

### Follow-up: Solution without strings

## My solution:

```Java
class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0) return false;
        if (x % 10 == 0 && x != 0) return false;
        
        int reverse_x = 0;
        while(x > reverse_x){
            reverse_x = reverse_x*10 + x%10;
            x /= 10;
        }
        return x == reverse_x || x == reverse_x/10;
    }
}
```
