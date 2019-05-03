# 2. Add Two Numbers
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example**:
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

## My Ans
```java
// - cur: points to the digits we're adding currently;
// - a: additional number from previous addition;
// - until cur points to null for l1 and l2 AND a == 0;
//
// Time: O(N)

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if (l1 == null && l2 == null) {
            return null;
        }
        else if (l1 == null || l2 == null) {
            return l1 == null ? l2 : l1;
        }
        ListNode res = new ListNode(-1);
        ListNode res0 = res;
        int a = 0, now = 0;
        while (l1 != null || l2 != null || a != 0) {
            if (l1 == null && l2 == null) {
                now = a;
            }
            else if (l1 == null) {
                now = l2.val + a;
                l2 = l2.next;
            }
            else if (l2 == null) {
                now = l1.val + a;
                l1 = l1.next;
            }
            else {
                now = l1.val + l2.val + a;
                l1 = l1.next;
                l2 = l2.next;
            }
            
            if (now >= 10) {
                res.next = new ListNode(now - 10);
                a = 1;
            }
            else {
                res.next = new ListNode(now);
                a = 0;
            }
            res = res.next;
        }
        return res0.next;
    }
}
```