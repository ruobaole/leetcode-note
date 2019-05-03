# 3. Longest Substring Without Repeating Characters
Given a string, find the length of the longest substring without repeating characters.

Example 1:
```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

Example 2:
```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

Example 3:
```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## My Ans
```java
// Opimized Sliding Window
// - [s, f) the sliding window of the substring;
// - Instead of using a HashSet, we use a HashMap<Character, Integer> with key: s[i], value: index of s[i];
// - Each time, if s[f] exists, we can jump s directly to map.get(s[f])+1; And replace map[s[f]] with f;
// - To determine if s[f] exists, we need to compare if map.get(s[f]) >= i;
//
// Time: O(N)
// Space: O(min(N, M)); N -- s.length(), M size of charset;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int res = 0;
        for (int left = 0, right = 0; right < s.length(); right++) {
            if (map.get(s.charAt(right)) != null && map.get(s.charAt(right)) >= left) {
                left = map.get(s.charAt(right)) + 1;
            }
            res = Math.max(res, right - left + 1);
            map.put(s.charAt(right), right);
        }
        return res;
    }
}
```