# priorityQueue
---

## 1. K-th Smallest in Unsorted Array
Input: [6, 2, -1, 3. 5, 8, 0], k = 3
Output: 2

```java
// Use maxHeap with size K too keep track of the k smallest elements we've met so far;
// Each time compare new element with maxHeap.peak() to see if we need to push it to the heap;
// Time: O((N+k)logk) (plus k --> poll out all elements in maxHeap)

public class Solution {
    public List<Integer> kSmallest(int[] array, int k) {
        if (array == null) {
            return null;
        }
        if (array.length < k || k <= 0) {
            return null;  // throw BadArgException
        }
        PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(k, new Comparator<Integer>() {
            @Override
            public int compare(Integer i, Integer j) {
                if (i.equals(j)) {
                    return 0;
                }
                return i > j ? -1 : 1;
            }
        });
        for (int i = 0; i < array.length; i++) {
            if (i < k) {
                maxHeap.offer(array[i]);
            }
            else if (array[i] < maxHeap.peak()) {
                maxHeap.poll();
                maxHeap.offer(array[i]);
            }
        }

        return this.queue2Array(maxHeap, array);
    }

    private List<Integer> queue2Array(PriorityQueue<Integer> queue) {
        List<Integer> res = new ArrayList<Integer>(queue.size());
        while (!queue.isEmpty()) {
            res.add(queue.poll());
        }
        return res;
    }
}
```

---
##2. K-th Smallest Number in Sorted Matrix
Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

###Example:
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
###Note: 
You may assume k is always valid, 1 ≤ k ≤ n2.

```java
// BestFS -- use a minHeap to find the next smallest element;
// Each time poll from the minHeap, expand its right and under neighbor (if not visited);
// Time: (klogk)
// Space: if use boolean visited[][] --> O(N*N) + O(k)
// O(k) if using HashSet;

class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        if (matrix == null) {
            throw new IllegalArgumentException("matrix should not be null");
        }
        int N = matrix.length;
        if (N == 0) {
            throw new IllegalArgumentException("matrix should have at least one element");
        }
        if (matrix[0] == null || (matrix[0] != null && matrix[0].length == 0)) {
            throw new IllegalArgumentException("matrix should have at least one element");
        }
        int M = matrix[0].length;
        if (k > N * M) {
            throw new IllegalArgumentException("k should be <= M*N");
        }
        
        PriorityQueue<Element> minHeap = new PriorityQueue<Element>(k, new Comparator<Element>() {
            @Override
            public int compare(Element e1, Element e2) {
                if (e1.val == e1.val) {
                    return 0;
                }
                return e1.val < e2.val ? -1 : 1;
            }
        });
        HashSet<Element> set = new HashSet<Element>();
        Element cur = new Element(0, 0, matrix[0][0]);
        minHeap.offer(cur);
        set.add(cur);
        int i = 0;
        while (i < k) {
            cur = minHeap.poll();
            if (cur.r+1 < N) {
                Element under = new Element(cur.r+1, cur.c, matrix[cur.r+1][cur.c]);
                if (set.add(under)) {
                    minHeap.offer(under);
                }
            }
            if (cur.c+1 < M) {
                Element right = new Element(cur.r, cur.c+1, matrix[cur.r][cur.c+1]);
                if (set.add(right)) {
                    minHeap.offer(right);
                }
            }
            i++;
        }
        return cur.val;
    }
    
    private class Element {
        int r;
        int c;
        int val;
        public Element(int r, int c, int val) {
            this.r = r;
            this.c = c;
            this.val = val;
        }
        
        @Override
        public boolean equals(Object obj) {
            if (this == obj) {
                return true;
            }
            if (!(obj instanceof Element)) {
                return false;
            }
            Element other = (Element) obj;
            return other.r == this.r && other.c == this.c;
        }
        
        @Override
        public int hashCode() {
            return this.r*31 + this.c;
        }
    }
}
```