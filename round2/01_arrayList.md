# Array List
## Conclusion
### 1 Rainbow sort
- In Rainbow sort or Partition(), N parts --> N borders;
- All pointers except the 2nd from the last one is determinated (i.e. we know
what it is pointing at)
- Only use the 2nd from the last pointer to detect the unkown part;
- In each iteration in while loop --> ONLY make decision based on what A[secondFromLastP] points at;
- Terminate condition of while loop --> l > r;

### 2 Search in a sorted array/matrix
- Find a trick so that in each iteration, you can eliminate as much of the elements.
(binary search -- dump half of the array; search in sorted matrix -- dump the 
whole row or col);

## 0.1 Merge Sort
Time: O(NlogN)
Space: O(N)

Instead of constructing and returning a new array at each level during 'merge', we pass the 
helperArray with parameter at first;
During each merge(), after mergeSort left and right half (array already sorted in-place), we copy
the array into helperArray, and merge the left and right half from helperArray into the input array;

```java
public class Solution {
    public ArrayList<Integer> mergeSort(ArrayList<Integer> input) {
        if (input == null) {
            return input;
        }
        ArrayList<Integer> helper = new ArrayList<Integer>(input.size());
        mergeSortHelper(input, helper, 0, input.size()-1);
        return input;
    }

    protected void mergeSortHelper(ArrayList<Integer> input, ArrayList<Integer> helper, int low, int high) {
        if (low >= high) {
            return;
        }
        int mid = low + (high - low)/2;
        mergeSortHelper(input, helper, low, mid);
        mergeSortHelper(input, helper, mid+1, high);

        // merge
        for (int i = 0; i < input.size(); i++) {
            helper[i] = input[i];
        }
        int left = low, right = mid+1, i = 0;
        while (left <= mid && right < high) {
            if (helper[left] < helper[right]) {
                input[i++] = helper[left++];
            }
            if (helper[right] <= helper[left]) {
                input[i++] = helper[right++];
            }
        }
        while (left <= mid) {
            input[i++] = helper[left++];
        }
    }
}

```

## 0.2 Quick Sort
Time: O(NlogN) -- worst case O(N^2) when each time the pivot goes to 0 or len-1;
Space: O(N) in-place
```java
public class Solution {
    public void quickSort(ArrayList<Integer> input) {
        if (input == null) {
            return;
        }
        quickSortHelper(input, 0, input.size()-1);
    }

    protected void quickSortHelper(ArrayList<Integer> input, int low, int high) {
        if (low >= high) {
            return;
        }

        int pivotIdx = this.partition(input, low, high);
        quickSortHelper(input, low, pivotIdx-1);
        quickSortHelper(input, pivotIdx+1, high);
    }

    protected int partition(ArrayList<Integer> input, int low, int high) {
        int pivotIdx = this.getRandom(low, high);
        int pivot = input[pivotIdx];
        this.swap(input, pivotIdx, high);
        int l = low, r = high-1;
        while (l <= r) {
            if (input[l] <= pivot) {
                l++;
            }
            else if (input[r] > pivot) {
                r--;
            }
            else {
                swap(input, l, r);
            }
        }
        swap(input, l, high);
        return l;
    }

    protected int getRandom(int low, int high) {
        return low + (int)(Math.random() * (high - low + 1));
    }

    protected void swap(ArrayList<Integer> input, int i, int j) {
        int tmp = input[i];
        input[i] = input[j];
        input[j] = tmp;
    } 
}

```





## 1. K Closest In Sorted Array
Input: A target integer T, non-negative integer K, an array of integer A sorted in ascending order;
Return: A size K array containing K closest numbers in A, sorted in ascending order by the difference between number and T.

Example:
A = {1, 2, 3}, T = 2, K = 3
return: {2, 1, 3} or {2, 3, 1}

A = {1, 4, 6, 8}, T = 3, K = 3
return: {4, 1, 6}

```java
// - Use binarySearch to find the closest element;
// - From the closest element, expanding left and right, compare
//   its distance to target and push into res;
//
// Time: O(logN + K)

public class Solution {
    public int[] kClosest(int[] A, int T, int K) {
        if (K <= 0 || A == null || A.length == 0) {
            return null;
        }
        int[] res = new int[K];
        int closestIdx = this.biSearchClosest(A, T, 0, A.length-1);
        res[0] = A[closestIdx];
        int l = closestIdx-1, r = closestIdx+1;
        int i = 1;
        while (i < K) {
            if (l > 0 && r < A.length) {
                if (Math.abs(A[l] - T) < Math.abs(A[r] - T)) {
                    res[i++] = A[l--];
                }
                else {
                    res[i++] = A[r++];
                }
            }
            else if (l > 0) {
                res[i++] = A[l--];
            }
            else if (r < A.length) {
                res[i++] = A[r++];
            }
            else {
                break;
            }
        }
        return res;
    }

    protected int biSearchClosest(int[] A, int target, int left, int right) {
        if (right - left <= 1) {
            if (Math.abs(A[left]-target) < Math.abs(A[right]-target)) {
                return left;
            }
            return right;
        }
        int mid = left + (right - left)/2;
        if (A[mid] == target) {
            return mid;
        }
        if (A[mid] > target) {
            return this.biSearchClosest(A, target, left, mid);
        }
        if (A[mid] < target) {
            return this.biSearchClosest(A, target, mid, right);
        }
    }
}
```

## 2. First Occurence
Given a target integer T and an integer array A sorted in ascending order, find the index of the first
occurence of T in A or return -1 if there is no such index;
Example:
A = {1, 2, 3} T = 2; return 1
A = {1, 2, 3} T = 4; return -1
A = {1, 2, 2, 3} T = 2; return 1

```java
// BinarySearch, instead of returning the index when A[mid] == target; move to [left, mid]
// subarray to continue finding the target

public class Solution {
    public int firstOccur(int[] A, int T) {
        if (A == null || A.length == 0) {
            return -1;
        }

        return this.binarySearchFirstOccur(A, T, 0, A.length-1);
    }

    protected int binarySearchFirstOccur(int[] A, int T, int left, int right) {
        if (right - left <= 1) {
            if (A[left] == T) {
                return left;
            }
            else if (A[right] == 1) {
                return A[right];
            }
            else {
                return -1;
            }
        }
        int mid = left + (right - left)/2;
        if (A[mid] >= target) {
            return this.binarySearchFirstOccur(A, T, left, mid);
        }
        else {
            return this.binarySearchFirstOccur(A, T, mid+1, right);
        }
    }
}

```

## 3. Dedupe Adjacent Repeated Elements in Sorted Array
Dedupe adjacent consecutive elements in sorted array, for the consecutive repeated elements, only retain one of them.
Return the length of the array after deduplication.

Example:
- Input: {1, 2, 3, 3, 3, 6, 6, 9}
- Output: {1, 2, 3, 6, 9} --> 5

- Input: {1, 1, 1}
- Output {1} --> 1

- Input: {}
- Output: {} ---> 0

```java
// 2 points -- slow, fast;
// - [0, slow] --> returning subarray
// - [fast, end] --> subarray to be examining
// - each time see if array[fast] == array[slow] --> fast++ | array[++slow] = array[fast++]

public class Solution {
    public int dedupeRepeat(int[] array) {
        if (array == null || array.length == 0) {
            return 0;
        }
        int slow = 0, fast = 1;
        while (fast < array.length) {
            if (array[fast] == array[slow]) {
                fast++;
            }
            else {
                array[++slow] = array[fast++];
            }
        }
        return slow+1;
    }
}

```

## 4. Middle Node of a Singly Linked List
- Input:  1 -> 2 -> 3 -> null
- Output:  2

- Input: 1 -> 2 -> 3 -> 4 -> null
- Output:  2

- Input: 1 -> 2 -> null
- Output: 1

- Input: 1 -> null
- Output: 1

- Input: null
- Output: null

```java
// slow, fast pointers
// - when slow++, fast += 2;
// - stop when fast == null || fast.next == null || fast.next.next = null

public class Solution {
    public ListNode findMid(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}

```

## 5. Insert Into Sorted LinkedList
- Input: 2 -> 3 -> 3 -> 6 -> 8 -> null,  5
- Output: 2 -> 3 -> 3 -> 5 -> 6 -> 8 -> null,

- Input: 2 -> 3 -> null, 1
- Output: 1 -> 2 -> 3 -> null

- Input: 2 -> 3 -> null, 8
- Output: 2 -> 3 -> 8 -> null,

- Input: null, 4
- Output: 4 -> null

```java
// Use dummy node to make your life easier

public class Solution {
    public ListNode insert(ListNode head, int newVal) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode newNode = new ListNode(newVal);
        ListNode prev = dummy;
        ListNode cur = head;
        while (cur != null && cur.val < newVal) {
            prev = cur;
            cur = cur.next;
        }
        prev.next = newNode;
        newNode.next = cur;
        return dummy.next;
    }
}
```

## 6. Merge 2 Sorted LinkedList
- 2 -> 3 -> 8 -> null
- 4 -> 6 -> 12 -> null
- Output: 2 -> 3 -> 4 -> 6 -> 8 -> 12 -> null;

- null
- 2 -> 6 -> 11 -> null
- Output: 2 -> 6 -> 11 -> null

```java
public class Solution {
    public ListNode mergeLinkedList(ListNode h1, ListNode h2) {
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        while (h1 != null && h2 != null) {
            if (h1.val < h2.val) {
                prev.next = h1;
                h1 = h1.next;

            }
            else {
                prev.next = h2;
                h2 = h2.next;
            }
            prev = prev.next;
        }
        if (h1 != null) {
            prev.next = h1;
        }
        if (h2 != null) {
            prev.next = h2;
        }
        return dummy.next;
    }
}

```

## 7. Rainbow Sort
input: ['a', 'b', 'a', 'b', 'c', 'b']
output: ['a', 'a', 'b', 'b', 'b', 'c' ]

```java
// 3 parts -> 3 border -> pa, pb, pc
// [0, pa-1] -- all 'a's
// [pa, pb] -- all 'b's;
// [pc+1, end] -- all 'c's;
// [pb, pc] -- all characters need to be detect;
// Use pb to detect the unkown parts --> switch case based on what pb points at;
//
// Time: O(N)
// Space: in-place

public class Solution {
    public void rainbowSort(char[] input) {
        // Assume input array contains no element other than 'a', 'b', or 'c';
        if (input == null || input.length == 0) {
            return;
        }
        int pa = 0, pb = 0, pc = input.length-1;
        while (pb <= pc) {
            if (input[pb] == 'b') {
                pb++;
            }
            else if (input[pb] == 'a') {
                swap(input, pb++, pa++);
            }
            else if (input[pb] == 'c') {
                swap(input, pb, pc--);
            }
        }
    }
}
```

## 8. Move all zero to ends
Input: [0, 1, 23, 4, 0, 33, 0, 18, 2]
Output: [1, 23, 4, 33, 18, 2, 0, 0, 0]
The order of non-zero elements should be remain;

```java
// 2 parts -> 2 border -> pnz, p0
// [0, pnz-1] all non-zero;
// [p0+1, end] all 0s;
// [pnz, p0] all unkown elements
// use pnz to detect the unkown parts and switch case based on pnz;

public class Solution {
    public void moveAllZerosEnd(int[] input) {
        if (input == null || input.length == 0) {
            return;
        }
        int pnz = 0, p0 = input.length-1;
        while (pnz <= p0) {
            if (input[pnz] == 0) {
                swap(input, pnz, p0--);
            }
            else {
                pnz++;
            }
        }
    }
}

```

## 9. Fibbonacci (leetcode 509)
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
// Iterative way
// Time: O(N)
// Space: O(1)

class Solution {
    public int fib(int N) {
        if (N == 0) {
            return 0;
        }
        if (N == 1) {
            return 1;
        }
        int pp = 0, p = 1, f = 1;
        for (int i = 2; i <= N; i++) {
            f = pp + p;
            pp = p;
            p = f;
        }
        return f;
    }
}

```

## 10. Pow(x, n) (leetcode 50)
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
// x^n = x^(n/2) * x^(n/2) -- n is even;
// x^n = x^(n/2) * x^(n/2) * x -- n is odd;
//
// Time: O(logN)
// Space: O(1)

public class Solution () {
    public double pow(double x, int n) {
        if (n > 0) {
            return powHelper(x, n);
        }
        if (n < 0) {
            return 1 / powHelper(x, n);
        }
        else {
            return 1;
        }
    }

    protected double powHelper(double x, int n) {
        double half = powHelper(x, n/2)
        if (n % 2 == 0) {
            return half * half;
        }
        else {
            return half * half * x;
        }
    }
}

```

## 11. Search a 2D matrix (leetcode 74)
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
// 1) use binary search to find the first element larger than target among last elements of
// all rows; -- which narrow down the target in that row;
// 2) in that row, use binary search to find the target;
//
// Time: O(logN + logM)

```