# HashMap
---

## 1. Find K Pairs with Smallest Sums (leetcode 373)
You are given two integer arrays nums1 and nums2 sorted in ascending order and an integer k.

Define a pair (u,v) which consists of one element from the first array and one element from the second array.

Find the k pairs (u1,v1),(u2,v2) ...(uk,vk) with the smallest sums.

###Example 1:
Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]] 
Explanation: The first 3 pairs are returned from the sequence: 
             [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
###Example 2:
Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
Output: [1,1],[1,1]
Explanation: The first 2 pairs are returned from the sequence: 
             [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
###Example 3:
Input: nums1 = [1,2], nums2 = [3], k = 3
Output: [1,3],[2,3]
Explanation: All possible pairs are returned from the sequence: [1,3],[2,3]

```java
// bestFS -- using a minHeap with size K;
// each time, poll from minHeap, push in its right and bottom neighbor (check if already visitd
// before push);
// If using boolean visited[][] -- Space: O(M * N) + O(K) --> not efficient if M*N >> k;
// Using HashSet --> Space: O(K);
// Time: O(klogk)

class Solution {
    public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        if (nums1 == null || nums2 == null) {
            return null;
        }
        int m = nums1.length, n = nums2.length;
        if (m * n < k) {
            k = m * n;
        }
        List<int[]>res = new ArrayList<int[]>(k);
        if (k == 0) {
            return res;
        }
        PriorityQueue<Pair> minHeap = new PriorityQueue<Pair>(k, new Comparator<Pair>() {
            @Override
            public int compare(Pair p1, Pair p2) {
                if (p1.sum == p2.sum) {
                    return 0;
                }
                return p1.sum < p2.sum ? -1 : 1;
            }
        });
        HashSet<Pair> set = new HashSet<Pair>();
        Pair cur = new Pair(0, 0, nums1[0]+nums2[0]);
        minHeap.offer(cur);
        set.add(cur);
        
        int i = 0;
        while (i < k) {
            cur = minHeap.poll();
            if (cur.u+1 < m) {
                Pair p1 = new Pair(cur.u+1, cur.v, nums1[cur.u+1]+nums2[cur.v]);
                if (set.add(p1)) {
                    minHeap.offer(p1);
                }
            }
            if (cur.v+1 < n) {
                Pair p2 = new Pair(cur.u, cur.v+1, nums1[cur.u]+nums2[cur.v+1]);
                if (set.add(p2)) {
                    minHeap.offer(p2);
                }
            }
            res.add(new int[] {nums1[cur.u], nums2[cur.v]});
            i++;
        }
        return res;
    }
    
    public class Pair {
        int u;
        int v;
        int sum;
        public Pair(int u, int v, int sum) {
            this.u = u;
            this.v = v;
            this.sum = sum;
        }
        
        @Override
        public boolean equals(Object obj) {
            if (this == obj) {
                return true;
            }
            if (!(obj instanceof Pair)) {
                return false;
            }
            Pair other = (Pair) obj;
            return this.u == other.u && this.v == other.v && this.sum == other.sum;
        }
        
        @Override
        public int hashCode() {
            return this.u*31*31 + this.v*31 + this.sum;
        }
    }
}
```