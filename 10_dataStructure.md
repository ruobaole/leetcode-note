#Data Structure
---

##1. Array Deduplicate (sorted)
Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new array.

###Example 1:
Input: [1,1,2],
Output: [1, 2]

###Example 2:

Input: [0,0,1,1,1,2,2,3,3,4],
Output: [0,1,2,3,4]

**Operate in-place**

```java
// 2 pointers;
// [0, slow] -- the result subarray;
// [fast, end] -- the subarray to be examined;
// if array[fast] == array[slow]: skip fast;
// slow inited to 0

class Solution {
    public int[] dedup(int[] array) {
        if (array == null || array.length <= 1) {
            return array;
        }
        int s = 0, f = 1;
        while (f < array.length) {
            if (array[f] == array[s]) {
                f++;
            }
            else if (array[f] != array[s]) {
                array[++s] = array[f++];
            }
        }
        return Arrays.copryOf(array, 0, s+1);
    }
}

```

##2. Array Deduplicate II (sorted, keep at most 2 duplicates)
Input: [0, 1, 1, 1, 2, 2, 3, 4, 4, 4, 4]
Output: [0, 1, 1, 2, 2, 3, 4, 4]

```java
// 2 pointers;
// [0, slow] -- the result subarray; [fast, end] -- the subarray to be detected;
// slow inited to 1 (keep 0, 1 at least)
// - each time, compare array[fast] with array[slow-1], fast++ if equals;
//   write to ++slow if not equals;

class Solution {
    public int[] dedupII(int[] array) {
        if (array == null || array.length <= 2) {
            return array;
        }
        int s = 1, f = 2;
        while (f < array.length) {
            if (array[f] == array[s-1]) {
                f++;
            }
            else {
                array[++s] = array[f++];
            }
        }
        return Arrays.copyOf(array, 0, s+1);
    }
}

```