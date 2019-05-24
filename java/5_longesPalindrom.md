# 5. Longest Palindromic Substring
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example 1:
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

Example 2:
```
Input: "cbbd"
Output: "bb"
```

---
```java
// Solution 1.
// - Induction Rule:
//   substr(s, e) is palin only when: substr(s+1, e-1) is palin && S[s+1] == S[e-1]
// - Base Case:
//   substr(s, s) = true; substr(s, s+1) = S[s] == S[s+1];
// From len = 1, 2, 3, ... N. check if substr(i, i+len-1) is palin; -- longest len
// --> result start, end;
// Time: O(N^2); Space: O(N^2)
//
// Solution 2.
// - To check if str is palin -- expand from the middle one or middle 2 (if len is even);
// - Thus, for a str with len = N, need to check 2N - 1 possible core of palin;
// - isPalin(s, i, j) returns the len of the longest palin centered at element
//   between s[i] and s[j];
// - iterate all 2N-1 possible core and return the longest palin;
// Time: O(N^2); Space: O(N)

class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        int start = 0, end = 1;
        for (int i = 0; i < s.length(); i++) {
            int len1 = isPalin(s, i, i);
            int len2 = isPalin(s, i, i+1);
            int len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1)/2;
                end = i + (len - (len-1)/2);
            }
        }
        return s.substring(start, end);
    }
    
    protected int isPalin(String s, int left, int right) {
        // return the length of the longest palin centered between
        // left, right
        int len = 0;
        while (left >= 0 && right < s.length()) {
            if (s.charAt(left) != s.charAt(right)) {
                break;
            }
            len = right == left ? 1 : len+2;
            left--;
            right++;
        }
        return len;
    }
}
```