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
## 2. Quick Sort
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