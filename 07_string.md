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
##4. Permutations II
Given a collection of numbers that might contain duplicates, return all possible unique permutations.

##Example:
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]

```java
// Use DFS to generate all possible answers and collect at leaves;
// - At each level, determine which element should be at position i by iterating through [i, nums.length-1]
// and swap each of them to position i;
// - In order to avoid duplicate answers, we need to avoid duplicates at each level;
// *Tricky: when swapping, the already sorted substring may became unsorted again -- e.g.
// 100009 --> 100900, thus it is imposible to sort once and skip all consecutive
// duplicates to avoid overall duplicate answers;
// - One Method is to not swap the array, but insert nums[i] into the answer array;
//   Use another array boolean used[nums.length] to keep track of if nums[i] is already used
//   in the current path;
//   Cause we don't reorder the nums[], we can sort it in advance and skip all consecutive dups;
//
//  Time:(O(NlogN) + N!)

class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (nums == null) {
            return res;
        }
        
        Arrays.sort(nums);
        boolean[] used = new boolean[nums.length];
        List<Integer> ans = new ArrayList<Integer>();
        helper(nums, used, ans, res);
        return res;
    }
    
    protected void helper(int[] nums, boolean[] used, List<Integer> ans, List<List<Integer>> res) {
        if (ans.size() == nums.length) {
            res.add(new ArrayList<Integer>(ans));
            return;
        }
        
        
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) {
                continue;
            }
            used[i] = true;
            ans.add(nums[i]);
            helper(nums, used, ans, res);
            int j = i;
            while (i < nums.length-1 && nums[i] == nums[i+1]) {
                i++;
            }
            ans.remove(ans.size()-1);
            used[j] = false;
        }
    }
}

```

---
##5. All Valid Permutation of Parenthesis (leetcode 22.)
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]

```java
// Use DFS.
// - At each level i, choosing between whether we sould add a '(' or ')';
// - Collect answers at leaves;
// To generate valid parenthesis: 1) if we still have '(' in hand, we can add a '(';
// 2) we can only add a ')' if the number of ')' we have in hands is larger than number of 
// '(' we have;
// - Cause we know the answer string's length in advance, and we do lots of append and remove
// at tail -- both StringBuilder and char[] is OK;
//

class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<String>();
        if (n == 0) {
            return res;
        }
        
        char[] ans = new char[n*2];
        helper(n, n, 0, ans, res);
        return res;
    }
    
    protected void helper(int left, int right, int idx, char[] ans, List<String> res) {
        if (left == 0 && right == 0) {
            res.add(new String(ans));
            return;
        }
        
        if (left > 0) {
            ans[idx] = '(';
            helper(left-1, right, idx+1, ans, res);
        }
        if (right > left) {
            ans[idx] = ')';
            helper(left, right-1, idx+1, ans, res);
        }
    }
}

```

---
##6. Coin Combinations
You are given coins of different denominations and a total amount of money. Write a function to output all combinations that make up that amount. You may assume that you have infinite number of each kind of coin.

###Example 1:
Input: amount = 5, coins = [1, 2, 5]
Output: [
    [0, 0, 1],
    [1, 2, 0],
    [3, 1, 0],
    [5, 0, 0]
]
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

###Example 2:
Input: amount = 3, coins = [2]
Output: []
Explanation: the amount of 3 cannot be made up just with coins of 2.

###Example 3:
Input: amount = 10, coins = [10] 
Output: [
    [1]
]

###Note:
You can assume that

0 <= amount <= 5000
1 <= coin <= 5000
the number of coins is less than 500
the answer is guaranteed to fit into signed 32-bit integer

```java
// Use DFS to generate all possible combinations and return count
// - At each level idx, choosing between how many number of coin[idx] should we add by looping thourgh
//  [0, target/coin[idx]]
// - Check if this combination will work and collect answers at leaves;


class Solution {
    public int change(int amount, int[] coins) {
        if (coins == null || coins.length == 0) {
            if (amount == 0) {
                return 1;
            }
            return 0;
        }
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        List<Integer> ans = new ArrayList<Integer>(coins.length);        
        helper(coins, amount, 0, ans, res);
        return res.size();
    }
    
    protected void helper(int[] coins, int amount, int idx, List<Integer> ans, List<List<Integer>> res) {
        if (idx == coins.length-1) {
            if (amount % coins[idx] == 0) {
                // check if this combination well work;
                ans.add(amount/coins[idx]);
                res.add(new ArrayList<Integer>(ans));
                ans.remove(ans.size()-1);
            }
            return;
        }
        
        for (int i = 0; i <= (int)amount/coins[idx]; i++) {
            ans.add(i);
            helper(coins, amount-i*coins[idx], idx+1, ans, res);
            ans.remove(ans.size()-1);
        }
    }
}

```
---

##7. Number of Coin Combinations (leetcode 518)
You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.

##Example 1:

Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

##Example 2:

Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
Example 3:

Input: amount = 10, coins = [10] 
Output: 1
 

Note:

You can assume that

0 <= amount <= 5000
1 <= coin <= 5000
the number of coins is less than 500
the answer is guaranteed to fit into signed 32-bit integer