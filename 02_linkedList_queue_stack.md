# LinkedList, Queue, Stack, Deque
---
##1. Implement Queue Using Stacks (leetcode 232)

Implement the following operations of a queue using stacks.

push(x) -- Push element x to the back of queue.
pop() -- Removes the element from in front of queue.
peek() -- Get the front element.
empty() -- Return whether the queue is empty.

#Example:
MyQueue queue = new MyQueue();
queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false

#Notes:
- You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
- Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
- You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

```java
// Use 2 stacks -- inStack, outStack
// Every time push an element, push into inStack;
// Every time pop an element, check if outStack is empty, if so,
// pop all from inStack and push into outStack and then pop the top;
// Time: amortized O(1)

class MyQueue {
    private Deque<Integer> inStack;
    private Deque<Integer> outStack;

    /** Initialize your data structure here. */
    public MyQueue() {
        inStack = new LinkedList<Integer>();
        outStack = new LinkedList<Integer>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        inStack.offerFirst(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public Integer pop() {
        if (this.empty()) {
            return null;
        }
        this.move2OutStack();
        return outStack.pollFirst();
    }
    
    /** Get the front element. */
    public Integer peek() {
        if (this.empty()) {
            return null;
        }
        this.move2OutStack();
        return outStack.peekFirst();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return inStack.isEmpty() && outStack.isEmpty();
    }
    
    protected void move2OutStack() {
        if (this.outStack.isEmpty()) {
            while (!this.inStack.isEmpty()) {
                this.outStack.offerFirst(this.inStack.pollFirst());
            }
        }
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */

```

---
#2. Stack with min() (leetcode 155)
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

#Example:
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.

```java
// We need a minStk to store the mins we already seen;
// When pop an element, compare it with the top of minStk,
// and see if we need to pop out the top;

class MinStack {
    private Deque<Integer> stack;
    private Deque<Integer> minStk;

    /** initialize your data structure here. */
    public MinStack() {
        stack = new LinkedList<Integer>();
        minStk = new LinkedList<Integer>();
    }
    
    public void push(int x) {
        stack.offerFirst(x);
        if (minStk.isEmpty() || minStk.peekFirst() >= x) {
            minStk.offerFirst(x);
        }
    }
    
    public void pop() {
        Integer tmp = stack.pollFirst();
        if (tmp != null && minStk.peekFirst() == tmp) {
            minStk.pollFirst();
        }
    }
    
    public int top() {
        return stack.peekFirst();
    }
    
    public int getMin() {
        return minStk.peekFirst();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */

```

---
#3. Reverse LinkedList
L = 12 -> 11 -> 3 -> 81 -> null
output: 81 -> 3 -> 11 -> 12 -> null

```java
// Use 3 pointers prev, cur, next --> reverse each pair of ListNode;
// Be careful of the terminate condition

/**
 * class ListNode {
 *   public int value;
 *   public ListNode next;
 *   public ListNode(int value) {
 *     this.value = value;
 *     next = null;
 *   }
 * }
 */
public class Solution {
    public ListNode reverse(ListNode head) {
        ListNode prev = null, cur = head, next = head;
        while (cur != null) {
            next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
}

```

---
#4. Middle Node of LinkedList
L = 11 -> 21 -> 14 -> 27 -> null;
return 21

L = 11 -> 21 -> null
return 11

L = 11 -> null
return 11

```java
// Use slow/fast pointers;
// Becareful of the terminate condition

public class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode s = head, f = head;
        while (f != null && f.next != null && f.next.next != null) {
            s = s.next;
            f = f.next.next;
        }
        return s;
    }
}

```

---
#5. Check if LinkedList Has a Circle

```java
// Use slow/fast pointers, if circle exists, slow/fast pointers
// will meet;
// The number of steps slow pointer takes between 2 meet is the length
// of circle;

public class Solution {
    public boolean hasCircle(ListNode head) {
        ListNode s = head, f = head;
        while (f != null && f.next != null) {
            s = s.next;
            f = f.next.next;
            if (s == f) {
                return true;
            }
        }
        return false;
    }
}

```

---
#6. Insert in Sorted LinkedList
Input: L = null; 21
Output: 21 -> null;

Input: L = 3 -> null; 21
output: 3 -> 21 -> null;

```java
// Use 2 pointers to traverse and find the right position for inserted element;
// Use dummyHead to simplify your work

public class Solution {
    public ListNode insert(ListNode head, int value) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode prev = dummy, cur = head;
        while (cur != null && cur.value < value) {
            prev = cur;
            cur = cur.next;
        }
        ListNode newNode = new ListNode(value);
        prev.next = newNode;
        newNode.next = cur;
        return dummy.next;
    }
}

```

---
#7. Merge 2 Sorted LinkedList
Input: 1 -> 21 -> null;
        3 -> 12 -> 25 -> 32 -> null;
Output: 1 -> 3 -> 12 -> 21 -> 25 -> 32 -> null;

```java
// 2 pointers to iterate 2 LinkedList, another pointer cur to construct new LinkedList;
// Use dummy head to simplify the construction of the new LinkedList;

public class Solution {
    public ListNode merge(ListNode head1, ListNode head2) {
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy, p1 = head1, p2 = head2;
        while (p1 != null && p2 != null) {
            if (p1.value < p2.value) {
                cur.next = p1;
                p1 = p1.next;
            }
            else {
                cur.next = p2;
                p2 = p2.next;
            }
            cur = cur.next;
        }
        if (p1 != null) {
            cur.next = p1;
        }
        if (p2 != null) {
            cur.next = p2;
        }
        return dummy.next;
    }
}

```

---
#8. Reorder LinkedList
Reorder the given singly-linked list 
N1 -> N2 -> N3 -> N4 -> … -> Nn -> null to be
N1- > Nn -> N2 -> Nn-1 -> N3 -> Nn-2 -> … -> null

## Examples
- L = null, is reordered to null
- L = 1 -> null, is reordered to 1 -> null
- L = 1 -> 2 -> 3 -> 4 -> null, is reordered to 1 -> 4 -> 2 -> 3 -> null
- L = 1 -> 2 -> 3 -> null, is reordred to 1 -> 3 -> 2 -> null

```java
// N1 -> N2 -> N3 -> ...
// Nn -> Nn-1 -> Nn-2 -> ....
// 1) find the middle point and break the List into 2;
// 2) reverse the right list
// 3) merge the left and right list

public class Solution {
    public ListNode reorder(ListNode head) {
        ListNode mid = this.findMiddle(head);
        ListNode left = head;
        ListNode right = this.reverse(mid.next);

        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        i = 0;
        while (left != null && right != null) {
            if (i % 2 == 0) {
                cur.next = left;
                left = left.next;
            }
            else {
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
            i++;
        }
        if (left != null) {
            cur.next = left;
        }
        if (right != null) {
            cur.next = right;
        }
        return dummy.next;
    }

    protected ListNode findMiddle(ListNode head) {
        ListNode s = head, f = head;
        while (f != null && f.next != null && f.next.next != null) {
            s = s.next;
            f = f.next.next;
        }
        return s;
    }

    protected ListNode reverse(ListNode head) {
        ListNode prev = null, cur = head, next = head;
        while (cur != null) {
            next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
}

```

---
#9. Partition LinkedList
Given a linked list and a target value T, partition it such that all nodes less than T are listed before the nodes larger than or equal to target value T. The original relative order of the nodes in each of the two partitions should be preserved.

## Examples
- L = 2 -> 4 -> 3 -> 5 -> 1 -> null, T = 3, is partitioned to 2 -> 1 -> 4 -> 3 -> 5 -> null

```java
// Break the LinkedList into 2 -- one larger than target and one smaller than target;
// Link the smaller list's tail to larger one's head;
// Use 1 pointer to traverse the LinkedList and 2 cur pointers to construct;

public class Solution {
    public ListNode partition(ListNode head, int target) {
        ListNode dummyS = new ListNode(0);
        ListNode dummyL = new ListNode(0);
        ListNode p = head, small = dummyS, large = dummyL;

        while (p != null) {
            if (p < target) {
                small.next = p;
                small = small.next;
            }
            else {
                large.next = p;
                large = large.next;
            }
            p = p.next;
        }
        small.next = dummyL.next;
        large.next = null;

        return dummyS.next;
    }
}

```

---
#Conclusion - LinkedList
- 1. Terminate condition of while loop: f != null, f.next != null, f.next.next != null --> use examples to determine; Based on if you use f.next within the loop;
- 2. DummyHead -- use dummyHead when insert into or merge the linkedList;