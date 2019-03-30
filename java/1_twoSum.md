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