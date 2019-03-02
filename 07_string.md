# String
---

##1. All Subsets
Given a set of distinct integers, nums, return all possible subsets (the power set).

Note: The solution set must not contain duplicate subsets.

##Example:
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

```java
// Generate all possible combinations using a tree;
// Method 1) In each level i, we generate all subsets with length i by looping from
// i:nums.length-1 and append nums[i] to the ans from parent level; -- collect answers
// at each level;
// Method 2) Collect answers at leaves; -- in each level i, diverge by determine if we 
// need to append nums[i] or not;
//
// Time: O(2^N) -- think about method2

class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (nums == null) {
            return res;
        }
        List<Integer> ans = new ArrayList<Integer>();
        // helper1(nums, 0, ans, res);
        helper2(nums, 0, ans, res);
        return res;
    }
        
    // Method 1
    protected void helper1(int[] nums, int end, List<Integer> ans, List<List<Integer>> res) {
        if (ans.size() == nums.length+1) {
            return;
        }
        
        List<Integer> ans1 = new ArrayList<Integer>(ans);
        res.add(ans1);
        for (int i = end; i < nums.length; i++) {
            ans.add(nums[i]);
            helper1(nums, i+1, ans, res);
            ans.remove(ans.size() - 1);
        }
    }
    
    // Method 2
    protected void helper2(int[] nums, int idx, List<Integer> ans, List<List<Integer>> res) {
        if (idx == nums.length) {
            res.add(new ArrayList<Integer>(ans));
            return;
        }
        
        helper2(nums, idx+1, ans, res);
        ans.add(nums[idx]);
        helper2(nums, idx+1, ans, res);
        ans.remove(ans.size() - 1);
    }
}

```

---
##2. All Subsets II
** Without dup answer **
Given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).

Note: The solution set must not contain duplicate subsets.

##Example:
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

```java
// Use DFS to generate all answers;
// In each level of the tree, we generate all answers with length i by iterating through i to
// nums.length()-1 and append nums[i] to the end of the answer from parent level;
// Collect ans at each level;
// In-order to dedup, we need to sort the array first and on each level, skips all the consequtive
// equal nums;
// *Tricky: When encounter duplicate consequtive elements, add first and then skip all consecutives;
// Because you need to care about situations when a is followed by a;
//
// Time: O(NlogN + 2^N)

class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (nums == null) {
            return res;
        }
        
        List<Integer> ans = new ArrayList<Integer>();
        Arrays.sort(nums);
        helper1(nums, 0, ans, res);
        return res;
    }
    
    protected void helper1(int[] nums, int end, List<Integer> ans, List<List<Integer>> res) {
        if (ans.size() > nums.length) {
            return;
        }
        
        res.add(new ArrayList<Integer>(ans));
        for (int i = end; i < nums.length; i++) {
            ans.add(nums[i]);
            helper1(nums, i+1, ans, res);
            ans.remove(ans.size()-1);
            // skip all consequtive dups
            while (i < nums.length-1 && nums[i] == nums[i+1]) {
                i++;
            }
        }
    }
}

```

---
##3. All Permutations

```java
// Use DFS to generate all answers;
// On each level i, we determine which element should be at position i by swapping all elements within
// [i, nums.length-1] to i;
// Collect answers at leaves;
// Time: O(N!)

class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (nums == null) {
            return res;
        }
        
        helper(nums, 0, res);
        return res;
    }
    
    protected void helper(int[] nums, int idx, List<List<Integer>> res) {
        if (idx == nums.length) {
            List<Integer> ans = new ArrayList<Integer>(nums.length);
            for (int e : nums) {
                ans.add(e);
            }
            res.add(ans);
            return;
        }
        
        for (int i = idx; i < nums.length; i++) {
            swap(nums, idx, i);
            helper(nums, idx+1, res);
            swap(nums, idx, i);
        }
    }
    
    protected void swap(int[] nums, int i, int j) {
        if (i == j) {
            return;
        }
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}

```

---
