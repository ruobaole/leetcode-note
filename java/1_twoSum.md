# 1. Two Sum
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

## Example:
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

```java
// Using a HashMap<Integer, Integer> key = target - e, value = index of e
// - Iterate through each element, adds (target - e, idx) into the map;
// - If we meet an element equals to one of the keys in the map, output [map.(e), idx]; 
//
// Time: O(N)
// Space: O(N)

class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length < 2) {
            return new int[]{};
        }
        
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])) {
                return new int[] {map.get(nums[i]), i};                
            }
            map.put(target - nums[i], i);
        }
        return new int[]{};
    }
}
```
---

# 2. Two Sum II (sorted) (l. 167)
Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

## Note:
Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.

## Example:
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.

```java
// 2 pointers left, right;
// - if sum < target, left++;
// - if sum > target, right--;
//
// Time: O(N)

class Solution {
    public int[] twoSum(int[] numbers, int target) {
        if (numbers == null || numbers.length < 2) {
            return new int[] {};
        }
        int left = 0, right = numbers.length-1;
        while (left < right) {
            if (numbers[left] + numbers[right] < target) {
                left++;
            }
            else if (numbers[left] + numbers[right] > target) {
                right--;
            }
            else {
                return new int[] {Math.min(left, right)+1, Math.max(left, right)+1};
            }
        }
        return new int[] {};
    }
}
```
---

# 3. Two Sum IV - input is a BST (l. 653)
Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.
## Example 1:
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
 

## Example 2:
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 28

Output: False

```java
// Same as twoSum in unsorted list;
// Use a set and perform DFS (any order would be ok);
//
// Time: O(N)
// Space: O(N)

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        if (root == null) {
            return false;
        }
        
        HashSet<Integer> set = new HashSet<Integer>();
        set.add(k - root.val);
        TreeNode findLeft = helper(root.left, k, set);
        if (findLeft != null) {
            return true;
        }
        TreeNode findRight = helper(root.right, k, set);
        if (findRight != null) {
            return true;
        }
        return false;
    }
    
    protected TreeNode helper(TreeNode root, int k, HashSet<Integer> set) {
        if (root == null) {
            return null;
        }
        if (set.contains(root.val)) {
            return root;
        }
        set.add(k - root.val);
        TreeNode findLeft = helper(root.left, k, set);
        if (findLeft != null) {
            return findLeft;
        }
        TreeNode findRight = helper(root.right, k, set);
        if (findRight != null) {
            return findRight;
        }
        return null;
    }
}
```
---

# 4. Subarray sum equals k (l.560)
Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

## Example 1:
Input:nums = [1,1,1], k = 2
Output: 2
Note:
The length of the array is in range [1, 20,000].
The range of numbers in the array is [-1000, 1000] and the range of the integer k is [-1e7, 1e7].

```java
// Use 2 pointers with HashMap<Integer, Integer> (key - sum, value - number of this sum we've seen)
// If subarray[i, j] has sum of k -> sum[0, j] - sum [0, i] = k -> sum[0, i] = sum[0, j] - k;
// - We can use a HashMap, this time, keep sum[0, i] in the set as we traverse from 0 to end;
// - Try to find sum[0, j] - k in the set;
// - Cause we may encounter [0, i1] and [0, i2] both have a sum of S --> we need HashMap'value to
//   keep track of number of sum we've encountered;
// *Tricky: don't forget to put (0, 1) in the map first;
//
// Time: O(N)
// Space: O(N)

class Solution {
    public int subarraySum(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        map.put(0, 1);
        int sum = 0, ans = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (map.containsKey(sum - k)) {
                ans += map.get(sum - k);
            }
            if (!map.containsKey(sum)) {
                map.put(sum, 1);
            }
            else {
                map.put(sum, map.get(sum) + 1);
            }
        }
        return ans;
    }
}
```
---

# 5. 3 Sum (l.15)
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:
The solution set must not contain duplicate triplets.

## Example:

Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

```java
// - Iterate from 0 to end-2; Each nums[i] is the candidate for 1st element in the
// triplets;
// - From [i+1, end], use standard way to find twoSum == 0 - nums[i];
// *Tricky:
// 1) To avoid duplicates, we need to sort the array first;
//    and then try to skip all duplicates when searching for 1st, 2nd, 3rd elements;
// 2) To not use extra space (HashSet) when doing twoSum, we also need to sort the array first;
//
// Time: O(N^2)

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (nums == null || nums.length < 3) {
            return res;
        }
        
        Arrays.sort(nums);
        for (int i = 0; i < nums.length-2; i++) {
            // skip all duplicate 1st elements
            if (i == 0 || nums[i] != nums[i-1]) {
                int left = i+1, right = nums.length-1;
                while (left < right) {
                    int sum = nums[left] + nums[right];
                    if (sum == 0 - nums[i]) {
                        res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                        // skip all duplicates if found one ans
                        while (left < right && nums[left] == nums[left+1]) {
                            left++;
                        }
                        while (left < right && nums[right] == nums[right-1]) {
                            right--;
                        }
                        left++;
                        right--;
                    }
                    if (sum < 0 - nums[i]) {
                        left++;
                    }
                    if (sum > 0 - nums[i]) {
                        right--;
                    }
                }
            }
        }
        return res;
    }
}

```

---
# 6. 4 Sum (l.18)
Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note:

The solution set must not contain duplicate quadruplets.

## Example:

Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]

```java
// Reduce to 2Sum
// - get 2sum of all pairs in array; store in HashMap<Integer, List<Integer>>
//   key -- sum of pair, value -- [i1, j1, i2, j2]
// - get 2sums of all pairs again, this time try to find (target - 2sum) in input HashSet;
// - To ensure the 2 pairs conform a valid quad, we only push to answer if j2 > j1; 
//   (i1 < j1 < j2 < i2)
// *Tricky: sort to avoid duplicate:
// - In first iteration, get 2 sums from left to right, skip all consequtive elements and
//  keep the first one; (so that among final answers, no 1st and 2nd elements are
//  the same)
// - In second iteration, skip all consequtive and keep the last one, so that we won't
//  miss situation where 1st == 3rd && 2nd == 4th elements;
//
// Time: O(N^2)

class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (nums == null || nums.length < 4) {
            return res;
        }

        HashMap<Integer, List<Integer>> map = new HashMap<Integer, List<Integer>>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length-1; i++) {
            // skip consequtive and keep 1st one
            if (i > 0 && nums[i] == nums[i-1]) {
                continue;
            }
            for (int j = i+1; j < nums.length; j++) {
                if (j > i+1 && nums[j] == nums[j-1]) {
                    continue;
                }
                int sum = nums[i] + nums[j];
                if (map.get(sum) == null) {
                    List<Integer> pairs = new ArrayList<Integer>();
                    map.put(sum, pairs);
                }
                map.get(sum).add(i);
                map.get(sum).add(j);
            }
        }

        for (int i = nums.length-1; i >= 1; i--) {
            if (i < nums.length-1 && nums[i] == nums[i+1]) {
                continue;
            }
            for (int j = i-1; j >= 0; j--) {
                if (j < i-1 && nums[j] == nums[j+1]) {
                    continue;
                }
                int sum2 = nums[i] + nums[j];
                if (map.get(target - sum2) != null) {
                    List<Integer> pairs = map.get(target - sum2);
                    for (int k = 0; k < pairs.size(); k += 2) {
                        if (pairs.get(k+1) < j) {
                            res.add(Arrays.asList(nums[pairs.get(k)], nums[pairs.get(k+1)], nums[j], nums[i]));
                        }
                    }
                }
            }
        }
        return res;
    }
}
```