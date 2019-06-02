# 12. Integer to Roman
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.
```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

Example 1:
```
Input: 3
Output: "III"
```
Example 2:
```
Input: 4
Output: "IV"
```
Example 3:
```
Input: 9
Output: "IX"
```
Example 4:
```
Input: 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```
Example 5:
```
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

```java
// Assume: num >= 1 && num <= 3999;
// - All build-up bricks: [1, 4, 5, 9, 10, 40, 50, 90, 100, 400, 500, 900, 1000];
// - Use 2 extra arrays to map integer build-up bricks to its roman strings;
// - From large to small, if (num >= brick[i]), append romanBrick[i], num -= brick[i];
// - Move to smaller brick if (num < brick[i])
//
// Space: O(1) --> num of bricks of roman

class Solution {
    public String intToRoman(int num) {
        if (num < 1 || num > 3999) {
            return "";
        }
        StringBuffer res = new StringBuffer();
        int[] brick = new int[] {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] brickRoman = new String[] {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        for (int i = 0; i < brick.length; i++) {
            while (num >= brick[i]) {
                res.append(brickRoman[i]);
                num -= brick[i];
            }
        }
        return res.toString();
    }
}
```

#13. Roman to Integer
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.
```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

Example 1:
```
Input: "III"
Output: 3
```
Example 2:
```
Input: "IV"
Output: 4
```
Example 3:
```
Input: "IX"
Output: 9
```
Example 4:
```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```
Example 5:
```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

```java
// Assume the input string is legal;
// - Just read from left to right, adding each integer;
// - Each time compare if isRomanSmaller(s[i], s[i+1]) --> we meet cases of 4 or 9; 

class Solution {
    public int romanToInt(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int res = 0, cur = 0;
        for (int i = 0; i < s.length(); i++) {
            cur = this.getRomanVal(s.charAt(i));
            if (i < s.length()-1 && this.isRomanSmaller(s.charAt(i), s.charAt(i+1))) {
                res -= cur;
            }
            else {
                res += cur;
            }
        }
        return res;
    }
    
    protected int getRomanVal(char c) {
        if (c == 'I') {
            return 1;
        }
        if (c == 'V') {
            return 5;
        }
        if (c == 'X') {
            return 10;
        }
        if (c == 'L') {
            return 50;
        }
        if (c == 'C') {
            return 100;
        }
        if (c == 'D') {
            return 500;
        }
        if (c == 'M') {
            return 1000;
        }
        return 0;
    }
    
    protected boolean isRomanSmaller(char c1, char c2) {
        if (c1 == 'I' && (c2 == 'V' || c2 == 'X')) {
            return true;
        }
        if (c1 == 'X' && (c2 == 'L' || c2 == 'C')) {
            return true;
        }
        if (c1 == 'C' && (c2 == 'D' || c2 == 'M')) {
            return true;
        }
        return false;
    }
}
```