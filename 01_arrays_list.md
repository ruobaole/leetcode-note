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