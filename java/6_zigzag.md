# 6. Zig Zag
https://leetcode.com/problems/zigzag-conversion/

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);
Example 1:
```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```
Example 2:
```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
```
---
## A not very good answer
```java
// Draw it on paper and try to find the model to calculate s_out[i];
// - delta = 2*R-2; numCol = s.length()/delta + 1;
// e.g. (index in input string)
// Row0: 0, 0+delta, 0+2*delta, ... 0+numCol*delta;
// Row1: 1, 1+delta-2, 1+delta, 1+2*delta-2, 1+2*delta, ...
// Row2: 2, 2+delta-2*2, 2+delta-2, 2+2*delta-2*2, 2+2*delta, ...
// Row3: 3, 3+delta-2*3 (if exists), 3+delta, 3+2*delta-2*3, 3+2*delta,...
//
// iterate through all rows, check existance for each position, concate the
// the string;
// Time: O(N), Space: O(N)

class Solution {
    public String convert(String s, int numRows) {
        if (s == null || s.length() == 0) {
            return "";
        }
        if (numRows == 1) {
            return s;
        }
        char[] res = new char[s.length()];
        int delta = 2 * numRows - 2;
        int C = (int) Math.ceil(s.length()/(float) delta) + 1;
        int i = 0;
        for (int r = 0; r < Math.min(numRows, s.length()); r++) {
            for (int c = 0; c < C; c++) {
                int core = r + c * delta;
                if (r > 0 && r < numRows-1) {
                    int corePrev = core - 2 * r;
                    if (corePrev >= 0 && corePrev < s.length()) {
                        res[i] = (s.charAt(corePrev));
                        i++;
                    }
                }
                if (core >= 0 && core < s.length()) {
                    res[i] = (s.charAt(core));
                    i++;
                }
            }
        }
        return new String(res);
    }
}
```