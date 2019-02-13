# Arrays List
---
##1. Merge Sort

```java
// Recursively divided the array into 2 halves, sort each of them
// and then merge the 2 halves;
// Time: O(NlogN)
// Space: We can pass a helper array into the merge() function; And in each
// recursion, we 1) copy the original array into the helper array; 2) merge from
// the helper array; -- allocate a fixed space O(N) in heap to reuse;

public class Solution {
    private void mergeSortHelper(int[] A, int left, int right, int[] helperA) {
        if (left >= right) {
            return;
        }

        int mid = left + (right - left)/2;
        mergeSortHelper(A, left, mid, helperA);
        mergeSortHelper(A, mid+1, right, helperA);

        // merge
        for (int i = left, i <= right, i++) {
            helperA[i] = A[i];
        }

        int l = left, r = mid+1, i = left;
        while (l <= mid && r <= right) {
            if (leftA[l] <= rightA[r]) {
                A[i++] = helperA[l++]; 
            }
            else {
                A[i++] = helperA[r++];
            }
        }
        while (l <= mid) {
            A[i++] = helperA[l++];
        }
    } 

    public int[] mergeSort(int[] A) {
        if (A == null) {
            return A;
        }

        int[] helperA = new int[A.length];
        mergeSortHelper(A, 0, A.length-1, helperA);
        return A;
    }
}

```
---

##2. Quick Sort
```java
// Each time we pick a pivot (randomly), put the pivot in its correct position
// by putting all elements <= pivote element to its left and all elements > pivote element
// to its right;
// We then recursively partition the left and right subarray of the pivot;
// Time: O(NlogN) -- worst case O(N^2) if the pivot in every recursion partition the array to
// A.length - 1 and 1;
// Space: in space sort;
//
// *Be careful of the ternimal condition of the while loop -- l <= r

public class Solution {
    private void partition(int[] A, int left, int right) {
        int pivot = int(Math.random() * (right - left + 1)) + left;
        int pivotEle = A[pivot];
        swap(A, pivot, right);
        
        int l = left, r = right - 1;
        while (l <= r) {
            if (A[l] <= pivotEle) {
                l++;
            }
            else if (A[r] > pivotEle) {
                r--;
            }
            else {
                swap(A, l++, r--);
            }
        }

        swap(A, l, right);
        return l;
    }

    private void quickSortHelper(int[] A, int left, int right) {
        if (left >= right) {
            return;
        }

        int pivot = partition(A, left, right);
        quickSortHelper(A, left, pivot-1);
        quickSortHelper(A, pivot+1, right);
    }

    public int[] quickSort(int[] A) {
        if (A == null) {
            return A;
        }

        quickSortHelper(A, 0, A.length-1);
        return A;
    }
}

``` 
---

##3. Rainbow Sort
input: ['a', 'b', 'a', 'b', 'c', 'b']
output: ['a', 'a', 'b', 'b', 'b', 'c' ]

```java
// Same as partition() in QuickSort,
// 3 part --> 3 borders --> pa, pb, pc
// all elements in range [0, pa-1] should be 'a's;
// [pa, pb-1] should be 'b's;
// [pc+1, A.length-1] should be 'c's;
// all elements within [pb, pc] are unkown;
// we should ONLY use pb to detect the unkown part of the array, and
// swich case based on A[pb];
//
// Time: O(N)
// in place partition;

public class Solution {
    public char[] rainbowSort(char[] A) {
        if (A == null || A.length == 0) {
            return A;
        }

        int pa = 0, pb = 0, pc = A.length-1;
        while (pb <= pc) {
            if (A[pb] == 'a') {
                swap(A, pb++, pa++);
            }
            else if (A[pb] == 'c') {
                swap(A, pb, pc--);
            }
            else {
                // A[pb] == 'b'
                pb++;
            }
        }
        return A;
    }

    private void swap(char[] A, int i, int j) {
        //...
    }
}

```
###Conclusion
- In Rainbow sort or Partition(), N parts --> N borders;
- All pointers except the 2nd from the last one is determinated (i.e. we know
what it is pointing at)
- Only use the 2nd from the last pointer to detect the unkown part;
- In each iteration in while loop --> ONLY make decision based on what A[secondFromLastP] points at;
- Terminate condition of while loop --> l > r;
---

##4. Move all zero to ends
Input: [0, 1, 23, 4, 0, 33, 0, 18, 2]
Output: [1, 23, 4, 33, 18, 2, 0, 0, 0]
The order of non-zero elements should be remain;

```java
// Partition into 2 parts -- 2 borders -- nz and z
// [0, nz-1] is all non-zeros; [z+1, A.length-1] all zeros;
// We can use nz as detector --> thus in each walking step, we should consider
// situtations nz meets with;
//
// Time: O(N)
// in space

public class Solution {
    public int[] moveAllZeroToEnd(int[] A) {
        if (A == null) {
            return A;
        }

        int nz = 0, z = A.length-1;
        while (nz <= z) {
            if (A[nz] == 0) {
                swap(A, nz, z--);
            }
            else {
                nz++;
            }
        }
        return A;
    }
}

```

---
##5. Fibbonacci (leetcode 509)
The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), for N > 1.
Given N, calculate F(N).

#Example 1:
Input: 2
Output: 1
Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.

#Example 2:
Input: 3
Output: 2
Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.

##Example 3:
Input: 4
Output: 3
Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.

#Note:
0 ≤ N ≤ 30.

```java
// Use DP with memo -- time O(N), space O(N);
// save time on overlapped caculation -- fib(i-2)
// M[N+1], M[i] -- Fib(i)
// Recursivly find fib(i) = fib(i-1) + fib(i-2)

class Solution {
    private int fibHelper(int[] M, int k) {
        if (k == 0) {
            M[k] = 0;
        }
        if (k == 1) {
            M[k] = 1;
        }
        if (M[k] != -1) {
            return M[k];
        }
        else {
            M[k] = fibHelper(M, k-1) + fibHelper(M, k-2);
        }
   
        return M[k];
    }
    
    public int fib(int N) {
        if (N < 0) {
            return 0;
        }
        int[] M = new int[N+1];
        for (int i = 0; i < N+1; i++) {
            M[i] = -1;
        }
        
        return fibHelper(M, N);
    }
}

```

---
##6. Pow(x, n) (leetcode 50)
Implement pow(x, n), which calculates x raised to the power n (xn).

#Example 1:
Input: 2.00000, 10
Output: 1024.00000

#Example 2:
Input: 2.10000, 3
Output: 9.26100

#Example 3:
Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25

#Note:
-100.0 < x < 100.0
n is a 32-bit signed integer, within the range [−231, 231 − 1]

```java
// x^n = x^(n/2) * x^(n/2)  -- n is even
// x^n = x^(n/2) * x^(n/2) * x -- n is odd
// Thus, each time we reduce the problem pow(x, n) to pow(x, n/2)
// by its half;
//
// Time: O(logN)
// Space: O(1)

class Solution {
    public double myPow(double x, int n) {
        if (n >= 0) {
            return myPowHelper(x, n);
        }
        else {
            return 1/myPowHelper(x, -1 * n);
        }
    }
    
    private double myPowHelper(double x, int k) {
        if (k == 0) {
            return 1;
        }
        double tmp = myPowHelper(x, k/2);
        if (k % 2 == 0) {
            return tmp * tmp;
        }
        return tmp * tmp * x;
    }
}

```

---
#7. First Occurence
Given a sorted array and a target number, find the index of the first occurence of the target number;
Return -1 if not found;
Input: [-9, 0, 1, 1, 2, 3, 3, 3, 5, 18], target: 2
output: 4

```java
// Use Binary Search
// In stead of returning the mid directly when A[mid] == target, continue to find within [left, mid]
// Stop on left == right - 1, compare A[left] and A[right]

public class solution {
    public int firstOccurence(int[] A, int target) {
        if (A == null) {
            return -1;
        }

        return binarySearch1(A, 0, A.length-1, target);
    }

    private int binarySearch1(int[] A, int left, int right, int target) {
        if (left >= right - 1) {
            if (A[left] == target) {
                return left;
            }
            if (A[right] == target) {
                return right;
            }
            return -1;
        }

        int mid = left + (left - right)/2;
        if (A[mid] >= target) {
            return binarySearch1(A, left, mid);
        }
        else {
            return binarySearch1(A, mid+1, right);
        }
    }
}

```

---
#8. K closest elements
Given a sorted array, two integers k and x, find the k closest elements to x in the array. The result should also be sorted in ascending order. If there is a tie, the smaller elements are always preferred.

#Example 1:
Input: [1,2,3,4,5], k=4, x=3
Output: [1,2,3,4]

#Example 2:
Input: [1,2,3,4,5], k=4, x=-1
Output: [1,2,3,4]

#Note:
The value k is positive and will always be smaller than the length of the sorted array.
Length of the given array is positive and will not exceed 10^4
Absolute value of elements in the array and x will not exceed 10^4

```java
// We need to return a subset of the input array with length k;
// --> use 2 pointers l, r, starting from 0 and A.length-1
// shrink the window in each step until the elements within [l, r]
// is the result (r - l + 1 == k)
//
// Time: O(N - k);

class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        if (arr == null || k > arr.length || arr.length == 0) {
            return null;
        }
        
        List<Integer> res = new ArrayList<>(k);
        int l = 0, r = arr.length-1;
        while (r - l + 1 > k) {
            if (Math.abs(arr[l] - x) > Math.abs(arr[r] - x)) {
                l++;
            }
            else {
                r--;
            }
        }
        
        for (int i = l; i <= r; i++) {
            res.add(arr[i]);
        }
        return res;
    }
}

```

##9. Search in Sorted Unkown Sized Array
/*
*  interface Dictionary {
*    public Integer get(int index);
*  }
*/

You do not need to implement the Dictionary interface.
You can use it directly, the implementation is provided when testing your solution

Given an unkown sized sorted array and a target number, 
return the index of the target number or -1 if not exists;

```java
// We need to find a range [left, right] that we can be sure target lies in that range;
// And start from the range, we can use binary search;
// To find the range, using single pointer right that doubles each time;

public class Solution {
    public int search(Dictionary dict, int target) {
        if (dict == null) {
            return -1;
        }

        int left = 0, right = 0;
        while (dict.get(right) !== null && dict.get(right) < target) {
            left = right;
            right *= 2;
        }

        return binarySearch(dict, left, right, target);
    }

    private int binarySearch(Dictionary dict, int left, int right, int target) {
        if (left > right) {
            return -1;
        }

        int mid = left + (right - left)/2;
        if (dict.get(mid) == target) {
            return mid;
        }
        if (dict.get(mid) == null || dict.get(mid) > target) {
            return binarySearch(dict, left, mid-1);
        }
        return binarySearch(dict, mid+1, right);
    }
}

```

##10. Search a 2D matrix (leetcode 74)
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.

#Example 1:
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true

#Example 2:
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false

```java
// Use binary search;
// We only need a function to transform index in the flattened
// array to row, col in the matrix;
// row = i / N;
// col = i + 1 - row * N - 1;
// Time: O(M * N)

class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        int M = matrix.length;
        if (matrix[0] == null || matrix[0].length == 0) {
            return false;
        }
        int N = matrix[0].length;
        
        return binarySearch(matrix, target, 0, M*N-1, M, N) != -1;
    }
    
    private int binarySearch(int[][] matrix, int target, int left, int right, int M, int N) {
        if (left > right) {
            return -1;
        }
        
        int mid = left + (right - left)/2;
        int[] rc = convert2RC(mid, M, N);
        if (matrix[rc[0]][rc[1]] == target) {
            return mid;
        }
        if (matrix[rc[0]][rc[1]] < target) {
            return binarySearch(matrix, target, mid+1, right, M, N);
        }
        return binarySearch(matrix, target, left, mid-1, M, N);
    }
    
    private int[] convert2RC(int idx, int M, int N) {
        int row = idx / N;
        int col = (idx + 1) - row * N - 1;
        int[] res = {row, col};
        return res;
    }
}
```

# Solution2
```java
// Solution 2. move row and then col
// Time: O(M + N)

class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        
        int M = matrix.length, N = matrix[0].length;
        int r = 0, c = N - 1;
        while (r < M && c >= 0) {
            if (matrix[r][c] == target) {
                return true;
            }
            if (matrix[r][c] < target) {
                r++;
            }
            else {
                c--;
            }
        }
        return false;
    }
}

```

##11. Search in Matrix II (leetcode 240)
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

#Example:
Consider the following matrix:

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]

Given target = 5, return true.
Given target = 20, return false.

```java
// Start from the last element of each row,
// if the target is larger than this element, eliminate the whole row
// if the target is smaller than this element, eliminate the whole col;
// Time: O(M+N)

class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int M = matrix.length, N = matrix[0].length;
        int r = 0, c = N - 1;
        while (r < M && c >= 0) {
            if (matrix[r][c] == target) {
                return true;
            }
            if (matrix[r][c] < target) {
                r++;
            }
            else {
                c--;
            }
        }
        return false;
    }
}

```

###Conclusion - Search in a sorted array/matrix
- Find a trick so that in each iteration, you can eliminate as much of the elements.
(binary search -- dump half of the array; search in sorted matrix -- dump the 
whole row or col);