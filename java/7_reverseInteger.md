#7. Reverse Integer
Given a 32-bit signed integer, reverse digits of an integer.

```
Example 1:
Input: 123
Output: 321
```
```
Example 2:
Input: -123
Output: -321
```
```
Example 3:
Input: 120
Output: 21
```
Note:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

```java
// - pop digit right -> left:
// pop = x % 10; x /= 10; until x == 0;
// - push digit:
// res = res * 10 + pop;
// - To check overflow:
// 1) if res > Integer.MAX_VALUE/10 --> return 0;
// 2) if res == Integer.MAX_VALUE/10 && pop > 7 --> return 0;
// 3) if res < Integer.MIN_VALUE/10 --> return 0;
// 4) if res == Integer.MIN_VALUE/10 && pop < -8 --> return 0;

class Solution {
    public int reverse(int x) {
        int res = 0, pop = 0;
        while (x != 0) {
            pop = x % 10;
            x /= 10;
            if (res > Integer.MAX_VALUE/10 || res == Integer.MAX_VALUE && pop > 7) {
                return 0;
            }
            if (res < Integer.MIN_VALUE/10 || res == Integer.MIN_VALUE && pop < -8) {
                return 0;
            }
            res = res * 10 + pop;
        }
        return res;
    }
}

```