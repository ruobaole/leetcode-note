# 1. Remove Duplicates from Sorted Array (l.26)
Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

Example 1:
```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

Example 2:
```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

Clarification:

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

Internally you can think of this:
```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

```java
// 2 pointers --> slow, fast;
// - [0, slow] the returning array;
// - [fast, end] the subarray being examining;
//
// Time: O(N)

class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int slow = 0, fast = 1;
        while (fast < nums.length) {
            if (nums[fast] == nums[slow]) {
                fast++;
            }
            else {
                nums[++slow] = nums[fast++];
            }
        }
        return slow+1;
    }
}
```

# 2. Remove Element (l. 27)

Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example 1:
```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```
Example 2:
```
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

```java
// 2 pointers -- slow, fast
// - [0, slow] --> returning subarray
// - [fast, end] --> the subarray being examined
// - if (array[fast] == val): fast++;
//
// Time: O(N)

class Solution {
    public int removeElement(int[] nums, int val) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int slow = -1, fast = 0;
        while (fast < nums.length) {
            if (nums[fast] == val) {
                fast++;
            }
            else {
                nums[++slow] = nums[fast++];
            }
        }
        return slow + 1;
    }
}
```

# 3. Product of Array Except (l.238)
Given an array nums of n integers where n > 1,  return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Example:
```
Input:  [1,2,3,4]
Output: [24,12,8,6]
Note: Please solve it without division and in O(n).
```
Follow up:
Could you solve it with constant space complexity? (The output array does not count as extra space for the purpose of space complexity analysis.)

```java
// N > 1 --> no obvious corner cases;
// Using an extra leftProduct[] and lastRight;
// - 1. iterate through nums left -> right, leftProduct[i] is the product
//   of all elements in [0, i);
// - 2. iterate through leftProduct[] right -> left, lastRight is the product
//   of all elements in (i, end];
// - 3. rewrite leftProduct[i] = leftProduct[i] * lastRight;
//      rewrite lastRight *= nums[i];
// return leftProduct[];
//
// Time: O(N);
// Space: O(N) -- no extra array;

class Solution {
    public int[] productExceptSelf(int[] nums) {
        if (nums == null || nums.length == 0) {
            return nums;
        }
        int[] leftProd = new int[nums.length];
        int lastRight = 1;
        for (int i = 0; i < nums.length; i++) {
            if (i == 0) {
                leftProd[i] = 1;
            }
            else {
                leftProd[i] = leftProd[i-1] * nums[i-1];
            }
        }
        for (int i = nums.length-1; i >= 0; i--) {
            leftProd[i] *= lastRight;
            lastRight *= nums[i];
        }
        return leftProd;
    }
}
```

# 4. Two Sum (l.1)

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
```java
// Assume:
// 1) only one result;
// 2) if no such pair --> return [-1, -1];
// Use an extra Map<Integer, Integer> key -- value of element, value -- index;
// - iterate nums, if (target-nums[i] in map), return [map.get(target-nums[i]), i];
//
// Time: O(N);
// Space: O(N);

class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[] {-1, -1};
        if (nums == null || nums.length < 2) {
            return res;
        }
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++) {
            if (map.get(target-nums[i]) != null) {
                res[0] = map.get(target-nums[i]);
                res[1] = i;
                return res;
            }
            else {
                map.put(nums[i], i);
            }
        }
        return res;
    }
}
```

# 5. Rotate Array (l.189)
Given an array, rotate the array to the right by k steps, where k is non-negative.

Example 1:
```
Input: [1,2,3,4,5,6,7] and k = 3
Output: [5,6,7,1,2,3,4]
```
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
Example 2:
```
Input: [-1,-100,3,99] and k = 2
Output: [3,99,-1,-100]
```
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
Note:

Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
Could you do it in-place with O(1) extra space?

```java
// Assume: k >= 0;
// Cyclic Replacement;
// - Rule: after rotate --> i1 = (i+(k % len)) % len;
// - from i = 0, make: tmp = nums[i1], nums[i1] = nums[i], i1 = 1;
// - this will place 0, k, 2k, ... elements into its correct position;
// - when i1 reached the starting position, starts from i1+1, which will 
//   continue to put i1+k, i1+2k, ... into correct position;
// - continue replacement until number of replacements == N
// Proof: N/k * k = N;
//
// Time: O(N);
// Space: in-place


class Solution {
    public void rotate(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return;
        }
        k = k % nums.length;
        int cnt = 0, start = 0, i = 0; 
        int tmp = 0, cur = 0;
        for (start = 0; cnt < nums.length; start++) {
            i = start;
            cur = nums[i];
            do {
                i = (i + k) % nums.length;
                tmp = nums[i];
                nums[i] = cur;
                cur = tmp;
                cnt++;
            } while (i != start);
        }
    }
}
```

# 6. Degree of an Array (l. 697)
Given a non-empty array of non-negative integers nums, the degree of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of nums, that has the same degree as nums.

Example 1:
```
Input: [1, 2, 2, 3, 1]
Output: 2
```
Explanation: 
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.

Example 2:
```
Input: [1,2,2,3,1,4,2]
Output: 6
```
Note:

nums.length will be between 1 and 50,000.
nums[i] will be an integer between 0 and 49,999.

```java
// - For an array with degree D, there must exists some number which occurs D times;
// - For each of the number, l -- its leftmost occurence idx; r -- its rigthmost
//   occurence idx --> subarray len = r - l + 1;
//
// - We can use 3 maps: freq (nums[i], freq), right (nums[i], its rightmost occurence idx),
//   left (nums[i], its leftmost occurence idx);
//
// Time: O(N)
// Space: O(N)

class Solution {
    public int findShortestSubArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        Map<Integer, Integer> freq = new HashMap<Integer, Integer>(),
        left = new HashMap<Integer, Integer>(),
        right = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++) {
            freq.put(nums[i], freq.getOrDefault(nums[i], 0)+1);
            if (left.get(nums[i]) == null) {
                left.put(nums[i], i);
            }
            right.put(nums[i], i);
        }
        int d = Collections.max(freq.values());
        int res = nums.length, len = 0;
        for (int x : freq.keySet()) {
            if (freq.get(x) == d) {
                len = right.get(x) - left.get(x) + 1;
                res = Math.min(res, len);
            }
        }
        return res;
    }
}
```

# 7. Maximum Distance to Closest Person (l. 849)
In a row of seats, 1 represents a person sitting in that seat, and 0 represents that the seat is empty. 

There is at least one empty seat, and at least one person sitting.

Alex wants to sit in the seat such that the distance between him and the closest person to him is maximized. 

Return that maximum distance to closest person.

Example 1:
```
Input: [1,0,0,0,1,0,1]
Output: 2
```
Explanation: 
If Alex sits in the second open seat (seats[2]), then the closest person has distance 2.
If Alex sits in any other open seat, the closest person has distance 1.
Thus, the maximum distance to the closest person is 2.

Example 2:
```
Input: [1,0,0,0]
Output: 3
```
Explanation: 
If Alex sits in the last seat, the closest person is 3 seats away.
This is the maximum distance possible, so the answer is 3.

Note:
```
1 <= seats.length <= 20000
seats contains only 0s or 1s, at least one 0, and at least one 1.
```

```java
// Assume: seats[] is valid
//
// 1. iterate left->right, get distLeft[i] -- distance of seats[i] to its closest neighbor
// on left;
// 2. iterate right->left, get distRight[i];
// - For cases when there're no person seats on its left or right side, just make 
// distRight[i] = distLeft[i] or Integer.MAX_VALUE
// 3. iterate each dist = min(distLeft[i], distRigth[i]), return the largets among all
// dist;
// - Of couse we can save the second array (distRight);
//
// Time: O(N)
// Space: O(N)

class Solution {
    public int maxDistToClosest(int[] seats) {
        if (seats == null || seats.length <= 1) {
            return -1;
        }
        int[] distLeft = new int[seats.length];
        int last = -1;
        for (int i = 0; i < seats.length; i++) {
            if (seats[i] == 1) {
                distLeft[i] = -1;
                last = i;
            }
            else if (last == -1) {
                distLeft[i] = Integer.MAX_VALUE;
            }
            else {
                distLeft[i] = i - last;
            }
        }
        last = -1;
        int distRight = -1, dist = -1, res = -1;
        for (int i = seats.length-1; i >= 0; i--) {
            if (seats[i] == 1) {
                distRight = -1;
                last = i;
            }
            else if (last == -1) {
                dist = distLeft[i];
                res = Math.max(res, dist);
            }
            else {
                distRight = last - i;
                dist = Math.min(distRight, distLeft[i]);
                res = Math.max(res, dist);
            }
        }
        return res;
    }
}
```