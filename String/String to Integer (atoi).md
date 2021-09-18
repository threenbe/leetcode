# String to Integer (atoi)

Implement atoi which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

## My solution:

```C
int myAtoi(char* str) {
    int ret = 0;
    int sign = 1;
    //discard whitespace chars
    while(*str == ' ') {
        str++;
        if (*str == '\0') return ret;
    }
    
    //signs
    if (*str == '-') {
        sign = -1;
        str++;
    } else if (*str == '+') {
        str++;
    }
    
    //numbers
    while (*str != '\0' && (*str >= '0' && *str <= '9')) {
        int add = (*str)-'0';
        if (ret > INT_MAX/10 || add > INT_MAX - ret*10){
            if (sign == 1) return INT_MAX;
            else return INT_MIN;
        }
        ret = ret*10 + add;
        str++;
    }
    return ret*sign;
}
```

## Another solution:

```Java
class Solution {
    public int myAtoi(String str) {
        boolean flag = false;
        boolean negative = false;
        boolean dontcheck = false;
        Integer ans = 0;
        //int mul = 1;
        for (int i = 0; i < str.length(); i++) {
            Character c = str.charAt(i);
            if (!flag){
                if (c.equals(' ')){
                    continue;
                } else if (!Character.isDigit(c)){
                    if (c.equals('-')){
                        negative = true;
                        flag = true;
                        dontcheck = true;
                    } else if (c.equals('+')){
                        flag = true;
                        dontcheck = true;
                    } else {
                        return 0;
                    }
                } else {
                    flag = true;
                }
            }
            if (flag && !dontcheck){
                if (Character.isDigit(c)){
                    Integer num = c - '0';
                    if (ans > Integer.MAX_VALUE/10){
                        if (negative) return Integer.MIN_VALUE;
                        else return Integer.MAX_VALUE;
                    }
                    if (!negative) {
                        if (num > Integer.MAX_VALUE - ans*10) {
                            return Integer.MAX_VALUE;
                        }
                    } else {
                        if (num*(-1) < Integer.MIN_VALUE - (ans*10*(-1))) {
                            return Integer.MIN_VALUE;
                        }
                    }
                    System.out.println(ans.toString() + "*10 + " + num.toString());
                    ans = ans*10 + num;
                } else {
                    break;
                }
            }
            dontcheck = false;
        }
        if (negative) return ans*(-1);
        else return ans;
    }
}
```

## Another solution:

```Java
class Solution {
    public int myAtoi(String s) {
        int res = 0;
        int i = 0;
        boolean first_digit_found = false;
        int polarity = 1;
        for (i = 0; i < s.length() && s.charAt(i) == ' '; i++) {};
        for (; i < s.length(); i++) {
            Character c = s.charAt(i);
            if (!Character.isDigit(c)) {
                if (!first_digit_found && c.equals('-')) {
                    first_digit_found = true;
                    polarity = -1;
                }
                else if (!first_digit_found && c.equals('+')) {
                    first_digit_found = true;
                }
                else {
                    break;
                }
            }
            else {
                first_digit_found = true;
                Integer num = c - '0';
                if (res > Integer.MAX_VALUE/10) {
                    if (polarity == 1) {
                        return Integer.MAX_VALUE;
                    }
                    else {
                        return Integer.MIN_VALUE;
                    }
                }
                if (polarity == 1 && num > Integer.MAX_VALUE - res*10) {
                    return Integer.MAX_VALUE;
                }
                if (polarity == -1 && num*-1 < Integer.MIN_VALUE + res*10) {
                    return Integer.MIN_VALUE;
                }
                res = res*10 + num;
            }
        }
        return res*polarity;
    }
}
```
