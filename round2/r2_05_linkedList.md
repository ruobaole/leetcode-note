# LinkedList
## 1. Remove Nth Node From End of List (l.19)
Given a linked list, remove the n-th node from the end of list and return its head.

Example:
```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```
Note:
Given n will always be valid.

Follow up:
Could you do this in one pass?

```java
// Use 3 pointers prev, cur, end;
// - 1. cur inited at head, move end = end.next, until cnt == n;
// - 2. move 3 pointers together until end == null;
// - 3. delete cur;
// Corner Cases:
// - head == null -> return null;
// - n > linkedlist length --> end == null when cnt still < n --> return head;
// *Use dummy head to save your life;

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy, cur = head, end = head;
        while (n > 0 && end != null) {
            end = end.next;
            n--;
        }
        if (end == null && n > 0) {
            // n > linkedList length
            return head;
        }
        while (end != null) {
            end = end.next;
            cur = cur.next;
            prev = prev.next;
        }
        prev.next = cur.next;
        cur.next = null;
        return dummy.next;
    }
}
```

## 2. Remove Duplicates from Sorted List II (l.82)
Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

Example 1:
```
Input: 1->2->3->3->4->4->5
Output: 1->2->5
```
Example 2:
```
Input: 1->1->1->2->3
Output: 2->3
```

```java
// slow fast pointers;
// - slow points to the tail of current subList without duplicates -- inited dummy;
// - fast points to the node to be examined -- inited head;
// - if fast != null && fast.next != null && fast.val == fast.next: continue
//   to move fast by fast = fast.next;
// - slow.next = fast.next, fast = fast.next, and start the next round;
// - until fast == null || fast.next = null;

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode slow = dummy, fast = head;
        while (fast != null && fast.next != null) {
            if (fast.val == fast.next.val) {
                while (fast != null && fast.next != null && fast.val == fast.next.val) {
                    fast = fast.next;
                }
                slow.next = fast.next;
                fast = fast.next;
            }
            else {
                slow = slow.next;
                fast = fast.next;
            }
        }
        return dummy.next;
    }
}
```

## 3. Linked List Cycle II (l.142)
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.
Note: Do not modify the linked list.

Example 1:
```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```
Example 2:
```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```
Example 3:
```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

```java
// slow, fast pointer;
// - 1) move 2 pointers until they meet;
// - 2 * (c_s * C + L + p) = c_f * C + L + p
// - p = (c_f - 2c_s)C - L --> L + p = (c_f - 2c_s)C -- integer times of C -->
// - from the meeting point, moves L steps, we can get to the cycle starting point;
// - 2) from the meeting point, let head and slow moves together one step by step until
// -   head == slow --> starting point;
// - if 2 pointers cannot meet at last, return -1;

/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode slow = head, fast = head;
        while (slow != null && fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                while (slow != head) {
                    slow = slow.next;
                    head = head.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```

## 4. Remove Linked List Elements (l.203)
Remove all elements from a linked list of integers that have value val.

Example:
```
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5
```

```java
// prev, cur pointer;
// - cur inited at head, prev inited at dummy;
// - move until cur.val = val: prev.next = cur.next, cur = cur.next;
// Corner Cases:
// - null
// - 6 -> null;

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy, cur = head;
        while (cur != null) {
            if (cur.val == val) {
                cur = cur.next;
            }
            else {
                prev.next = cur;
                prev = cur;
                cur = cur.next;
            }
        }
        prev.next = null;
        return dummy.next;
    }
}
```

## 5. Reverse Linked List (l.206)

Reverse a singly linked list.

Example:
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```
Follow up:
A linked list can be reversed either iteratively or recursively. Could you implement both?

```java
// Iterative: 
// - prev, cur, next pointers;
// - prev inited at null, cur, next inited at head;
// - reverse each pair of cur and prev;
// - Corner Case: 1 -> null
// Recursive:
// - recursively reverse(cur.next), return head (cur.next became tail);
// - Deduction: cur.next.next = cur; cur.next = null; return newHead;
// - Base Case: 1 -> null -- return;

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        // // Iterative
        // return reverseListIter(head);
        // Recursive
        if (head == null) {
            return head;
        }
        return reverseListRecur(head);
    }
    
    protected ListNode reverseListIter(ListNode head) {
        if (head == null) {
            return head;
        }
        ListNode prev = null, cur = head, tmp = cur;
        while (cur != null) {
            tmp = cur.next;
            cur.next = prev;
            prev = cur;
            cur = tmp;
        }
        return prev;
    }
    
    protected ListNode reverseListRecur(ListNode head) {
        if (head.next == null) {
            return head;
        }
        ListNode newHead = reverseListRecur(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```

## 6. Find the Duplicate Number (l.287)
Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.
Example 1:
```
Input: [1,3,4,2,2]
Output: 2
```
Example 2:
```
Input: [3,1,3,4,2]
Output: 3
```
Note:
- You must not modify the array (assume the array is read only).
- You must use only constant, O(1) extra space.
- Your runtime complexity should be less than O(n2).
- There is only one duplicate number in the array, but it could be repeated more than once.

```java
// Assume: only one duplicate number;
// Proof using 'pigeon hole';
// Same as LinedList Cycle;
// - if we make nums[i]'s next points to nums[nums[i]], we can then form a linked list;
// - 1) cause nums[i] are in range 1 ~ n, nums[i] must points to somewhere in nums;
// - 2) cause there are no 0 in nums, we start from nums[0] is the same as start from
//   the head of a LinkeList with cycle;
// - 3) use slow, fast pointer to find the starting point of the cycle --> the repeated
//   number;

class Solution {
    public int findDuplicate(int[] nums) {
        // Assume only one duplicate and nums.length >= 2;
        int slow = 0, fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        int h = 0;
        while (h != slow) {
            h = nums[h];
            slow = nums[slow];
        }
        return h;
    }
}
```