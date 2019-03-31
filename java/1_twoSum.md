# 1. Two Sum
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

##Example:
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

#2. Two Sum II (sorted) (l. 167)
Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

##Note:
Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.

##Example:
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

#3. Two Sum IV - input is a BST (l. 653)
Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.
##Example 1:
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
 

##Example 2:
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

#4. Subarray sum equals k (l.560)
Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

##Example 1:
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