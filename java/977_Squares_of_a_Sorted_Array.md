# 977. Squares of a Sorted Array

```java
// Assume squares of elements are no larger than Integer.Max;
// Two pointers, traverse from the left and right bound of array (which has the largest abs value)
// Each time compare A[left] and A[right]

class Solution {
    public int[] sortedSquares(int[] A) {
        if (A == null || A.length == 0) {
            return A;
        }
        int[] res = new int[A.length];
        int i = A.length - 1;
        int l = 0, r = A.length - 1;
        
        while (i >= 0) {
            if (A[l] * A[l] > A[r] * A[r]) {
                res[i--] = A[l] * A[l++];
            }
            else {
                res[i--] = A[r] * A[r--];
            }
        }
        return res;
    }
}
```