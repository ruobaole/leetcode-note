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
---

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
---

##3. Array Deduplicate III (sorted, keep no duplicated elements)
- Input: [0, 0, 0, 1, 2, 2, 3, 4, 4, 5]
- Output: [1, 3, 5]

- Input: [0, 0, 0]
- Output: []

```java
// 2 pointers -- same as ##4.

```


##4. Array Deduplicate VI (unsorted, remove all adjacent duplication repeatedly)
- Input: [2, 2, 2, 4, 6, 8, 8, 9, 9]
- Output: [4, 6]

- Input: [2, 3, 3, 3, 3]
- Output: [2]

```java
// 2 pointers
// [0, slow] -- the result subarray, [fast, end] -- subarray to be detected
// slow -- inited to -1; top of the stack
// - push to stack if slow == -1 || (slow >= 0) && array[fast] != array[slow]
// - skip all adjacent duplications if array[fast] == array[slow]; then slow-- --> pop top;

class Solution {
    public int[] dedupVI(int[] array) {
        if (array == null || array.length == 0) {
            return array;
        }

        int s = -1, f = 0;
        while (f < array.length) {
            if (s == -1 || (s >= 0 && array[f] != array[s])) {
                array[++s] = array[f++];
            }
            else if (array[f] == array[s]) {
                while (f < array.length && array[f] == array[s]) {
                    f++;
                }
                s--;
            }
        }
        if (array.length > 1 && s == 0) {
            s--;
        }
        return Arrays.copyOf(array, 0, s+1);
    }
}

```
---

##5. Rotate Square Matrix (l. 48)
You are given an n x n 2D matrix representing an image.
Rotate the image by 90 degrees (clockwise).

###Note:
You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

###Example 1:
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]

###Example 2:
Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]

```java
// Step1. mirror the matrix along y-axis;
// Step2. mirror the matrix along x==-y axis (r + c == N-1);

class Solution {
    public void rotate(int[][] matrix) {
        // Assume matrix is N * N square
        if (matrix == null || matrix.length == 0) {
            return;
        }
        int N = matrix.length;
        if (matrix[0] == null || matrix[0].length == 0) {
            return;
        }
        this.mirror(matrix, N);
        this.mirrorAlongDiagonal2(matrix, N);
    }
    
    protected void mirror(int[][] mat, int N) {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N/2; j++) {
                this.swap(mat, i, j, i, N-1-j);
            }
        }
    }
    
    protected void mirrorAlongDiagonal2(int[][] mat, int N) {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j + i < N-1; j++) {
                this.swap(mat, i, j, N-1-j, N-1-i);
            }
        }
    }
    
    protected void swap(int[][] mat, int i1, int j1, int i2, int j2) {
        int tmp = mat[i1][j1];
        mat[i1][j1] = mat[i2][j2];
        mat[i2][j2] = tmp;
    }
}

```