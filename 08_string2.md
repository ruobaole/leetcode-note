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