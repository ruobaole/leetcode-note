# Strings II
---

##1. Top k Frequent Words
Given a non-empty list of words, return the k most frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

##Example 1:
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]
Explanation: "i" and "love" are the two most frequent words.
    Note that "i" comes before "love" due to a lower alphabetical order.
#
Example 2:
Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words,
    with the number of occurrence being 4, 3, 2 and 1 respectively.

##Note:
You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
Input words contain only lowercase letters.
Follow up:
**Try to solve it in O(n log k) time and O(n) extra space.**

```java
// - Use HashMap<String, Integer> map to keep record all words and its frequencies;
// - Iterating through map.entrySet() with a minHeap<Map.Entry<String, Integer>> of 
// size k;
// If map.get(str) is larger than minHeap.peek(): minHeap.poll() and push str and its
// freq into minHeap;
// - poll out from minHeap one by one to get the result;
//
// Time: O(Nlogk)
// Space: O(N)

class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        List<String> res = new ArrayList<String>();
        if (k <= 0 || k > words.length) {
            return res;
        }
        
        HashMap<String, Integer> map = getFreqMap(words);
        PriorityQueue<Map.Entry<String, Integer>> minHeap = new PriorityQueue<Map.Entry<String, Integer>> (k, new Comparator<Map.Entry<String, Integer>>() {
           @Override
            public int compare(Map.Entry<String, Integer> e1, Map.Entry<String, Integer> e2) {
                if (e1.getValue().equals(e2.getValue())) {
                    return e1.getKey().compareTo(e2.getKey());
                }
                return e1.getValue().compareTo(e2.getValue());
            }
        });
        for (Map.Entry<String, Integer> e : map.entrySet()) {
            if (minHeap.size() < k) {
                minHeap.offer(e);
            }
            else if (e.getValue() >= minHeap.peek().getValue()) {
                minHeap.poll();
                minHeap.offer(e);
            }
        }
        
        this.minHeapToList(minHeap, res);
        return res;
    }
    
    protected HashMap<String, Integer> getFreqMap(String[] words) {
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        for (String s : words) {
            Integer freq = map.get(s);
            if (freq == null) {
                map.put(s, 1);
            }
            else {
                map.put(s, freq+1);
            }
        }
        return map;
    }
    
    protected void minHeapToList(PriorityQueue<Map.Entry<String, Integer>> minHeap, List<String> res) {
        while (!minHeap.isEmpty()) {
            res.add(minHeap.poll().getKey());
        }
        Collections.reverse(res);
    }
}

```
---

##2. Missing Number
Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

##Example 1:
Input: [3,0,1]
Output: 2

##Example 2:
Input: [9,6,4,2,3,5,7,0,1]
Output: 8

##Note:
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

```java
// Method 1. Using HashSet;
// Method 2. By Calculate Sum;
// Method 3. Using xor operation (x ^ x == 0)

class Solution {
    public int missingNumber(int[] nums) {
        return method3(nums);
    }
    
    protected int method3(int[] nums) {
        // Using xor
        int xor = 0;
        int n = nums.length;
        for (int i = 1; i <= n; i++) {
            xor ^= i;
        }
        for (int k : nums) {
            xor ^= k;
        }
        return xor;
    }
    
    protected int method2(int[] nums) {
        // Method2. Sum
        int n = nums.length;
        int sum = (0 + n) * (n + 1) / 2;
        int actualSum = 0;
        for (int i : nums) {
            actualSum += i;
        }
        return sum - actualSum;
    }
}

```
---

##3. Common Numbers of 2 Sorted Arrays
- Input:
[1, 4, 5, 18, 28, 30]
[4, 6, 18]
- Output:
[4, 18]

```java
// Use 2 pointers p1, p2;
// - p1++ if array[p1] < array[p2];
// - p2++ if array[p2] < array[p1];

class Solution {
    public List<Integer> commonNum(int[] array1, int[] array2) {
        List<Integer> res = new ArrayList<Integer>();
        if (array1 == null || array2 == null) {
            return res;
        }

        int p1 = 0, p2 = 0;
        while (p1 < array1.length && p2 < array2.length) {
            if (array1[p1] == array2[p2]) {
                res.push(array1[p1]);
                p1++;
                p2++;
            }
            else if (array1[p1] < array2[p2]) {
                p1++;
            }
            else if (array2[p2] < array1[p1]) {
                p2++;
            }
        }
        return res;
    }
}

```
---
##4. Remove Adjacent Repeated Chars
Input: 'abbbccdeeff'
Output: 'abcdef

```java
// Use 2 pointers;
// [0, s] is all char in the result string;
// f points to next char we need to examine;
// f++ if input.charAt(f) == input.charAt(s)

class Solution {
    public String deDup(String input) {
        if (input == null || input.length == 0) {
            return input;
        }
        int s, f = 0;
        char[] str = input.toCharArray();
        while (f < str.length) {
            if (str[f] == str[s]) {
                f++;
            }
            else {
                str[++s] = str[f++];
            }
        }
        return new String(str, 0, s+1);
    }
}
```
---
##5. Remove all Adjacent Repeated Chars
Input: 'abbbcddeef'
Output: 'acf

Input: 'aaa'
Output: ''

```java
// Use 2 pointers;
// [0, s] is the result string --> we can't be sure whether str[0] is in the result string or not
// thus, s is inited to -1;
// f points to the next char we need to examine;

class Solution {
    public String deDupII(String input) {
        if (input == null || input.length == 0) {
            return input;
        }
        char[] str = input.toCharArray();
        int s = -1, f = 0;
        while (f < str.length) {
            if (s < 0 || str[f] != str[s]) {
                str[++s] = str[f++];
            }
            else {
                while (f < str.length && str[s] == str[f]) {
                    f++;
                }
                s--;
            }
        }
        return new String(str, 0, s+1);
    }
}

```
---

##6. Determine If Substring
Input: 'mameemowmoo' 'mow'
Output: 5

return -1 if not substring

```java
// Iterate through largeStr's [0, large.length-small.length]
// Compare smallStr and largeStr[i, i+smallStr.length-1]
//
// Time: worst O((Nlarge - Nsmall) * Nsmall)

class Solution {
    public int isSubstring(String large, String small) {
        if (small == null || small.length == 0) {
            return 0;
        }
        if (large == null || large.length < small.length) {
            return -1;
        }

        int Nl = large.length, Ns = small.length;
        for (int i = 0; i <= Nl - Ns; i++) {
            if (compareStr(small, large, i)) {
                return i;
            }
        }
        return -1;
    }

    protected boolean compareStr(String small, String large, int start) {
        for (int i = 0; i < small.length; i++) {
            if (small.charAt(i) != large.charAt(start+i)) {
                return false;
            }
        }
        return true;
    }
}

```
---

##7. Remove all Spaces (leading/traling/duplicated)
- Input: "  an    apple a   day  "
Output: "an apple a day"

- Input: "   "
output: ""

```java
// 2 pointers; slow points to the last char in the result string;
// - Cause we're not sure whether the first letter should be in the result array or not --> slow is inited to -1;
// - 1) f --> skip all leading spaces --> if s == -1 and str[f] == ' ' --> f++
// - 2) for all dup and trailing spaces --> if str[f] == ' ' && str[s] == str[f] --> f++ --> (write 2st space and skip all others)
// - 3) for trailing spaces --> if str[s] points to ' ' at last, s--;

class Solution {
    public String removeAllSpaces(String input) {
        if (input == null || input.length == 0) {
            return input;
        }
        char[] str = input.toCharArray();
        int s = -1, f = 0;
        while (f < input.length) {
            if (s == -1) {
                if (str[f] == ' ') {
                    f++;
                }
                else {
                    str[++s] = str[f++];
                }
            }
            else if (str[s] == ' ' && str[f] == ' ') {
                f++;
            }
            else {
                str[++s] = str[f++];
            }
        }
        if (s >= 0 && str[s] == ' ') {
            s--;
        }
        return new String(str, 0, slow+1);
    }
}

```
---

##8. Remove Certain Chars From String
- Input: "kacikawawa is great" "ktc"
Output: "aiww is grea"

- Input: " an apple a day" "ktc"
Output: " an apple a day"

- Input: "play on the finger board" "kc"
Output: "paly on the finger board"

```java
// User 2 pointers and a hashset;
// - [0, slow] is the current returning substring;
// - [fast, end] is the substring waiting to be check;
// - turn string t into a hashset<Character> so that we can check if a char is in t in O(1)
//
// Time: O(M + N)

class Solution {
    public String removeCertainChar(String input, String t) {
        if (input == null || t == null || input.length == 0 || t.length == 0) {
            return input;
        }
        char[] str = input.toCharArray();
        HashSet<Character> tSet = getSet(t);
        int s = -1, f = 0;
        while (f < input.length) {
            if (tSet.contains(str[f])) {
                f++;
            }
            else {
                str[++s] = str[f++];
            }
        }
        return new String(str, 0, s+1);
    }

    protected HashSet<Character> getSet(String t) {
        HashSet<Character> res = new HashSet<Character>();
        for (int i = 0; i < t.length; i++) {
            res.add(t.charAt(i));
        }
        return res;
    }
}

```