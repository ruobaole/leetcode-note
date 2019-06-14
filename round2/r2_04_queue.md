# Queue

## Operation

- push
- pop
- size
- head/tail

```
class mqueue
mqueue.size = ()
mqueue.a = []
mqueue.head = null
mqueue.tail = null
```

## Implementation

- Using LinkedList
- Using cyclic array

---

## 1. Implement Stack Using Queues (l.225)
Implement the following operations of a stack using queues.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
empty() -- Return whether the stack is empty.
Example:
```
MyStack stack = new MyStack();

stack.push(1);
stack.push(2);  
stack.top();   // returns 2
stack.pop();   // returns 2
stack.empty(); // returns false
```
Notes:

You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.
Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).

```java
// Use 2 queues: pushQueue and popQueue 
// popQueue always has 0 or 1 element
// - push --> 1) if popQueue is not empty, pop and push it to pushQueue
//   2) push to pushQueue;
// - pop --> 1) if popQueue has one element, pop it out; 2) else, pop all from
//   pushQueue except the last one, and push them into popQueue; swich the pointer
//   of pop and pushQueue;
// - top --> same as pop, except we don't pop that one out;
//
// Time: pop, top --> O(N), push --> O(1)

class MyStack {
    protected Deque<Integer> pushQ;
    protected Deque<Integer> popQ;

    /** Initialize your data structure here. */
    public MyStack() {
        pushQ = new LinkedList<Integer>();
        popQ = new LinkedList<Integer>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        if (!popQ.isEmpty()) {
            pushQ.offerLast(popQ.pollFirst());
        }
        pushQ.offerLast(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        if (this.empty()) {
            throw new NullPointerException("Stack is empty");
        }
        int res = 0;
        if (!popQ.isEmpty()) {
            res = popQ.pollFirst();
            return res;
        }
        while (!pushQ.isEmpty()) {
            res = pushQ.pollFirst();
            if (pushQ.isEmpty()) {
                this.swapQueues();
                break;
            }
            popQ.offerLast(res);
        }
        return res;
    }
    
    /** Get the top element. */
    public int top() {
        if (this.empty()) {
            throw new NullPointerException("Stack is empty");
        }
        int res = 0;
        if (!popQ.isEmpty()) {
            res = popQ.peekFirst();
            return res;
        }
        while (pushQ.size() > 1) {
            res = pushQ.pollFirst();
            popQ.offerLast(res);
        }
        res = pushQ.peekFirst();
        this.swapQueues();
        return res;
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return pushQ.isEmpty() && popQ.isEmpty();
    }
    
    protected void swapQueues() {
        Deque<Integer> tmp = pushQ;
        pushQ = popQ;
        popQ = tmp;
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

## 2. Implement Queue Using Stacks (l.232)

Implement the following operations of a queue using stacks.

push(x) -- Push element x to the back of queue.
pop() -- Removes the element from in front of queue.
peek() -- Get the front element.
empty() -- Return whether the queue is empty.
Example:
```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

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
    public int pop() {
        if (this.empty()) {
            throw new NullPointerException("Queue is empty");
        }
        this.move2OutStack();
        return outStack.pollFirst();
    }
    
    /** Get the front element. */
    public int peek() {
        if (this.empty()) {
            throw new NullPointerException("Queue is empty");
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

## 3. Sliding Window Maximum (l.239)

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

Example:
```
Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
Note: 
You may assume k is always valid, 1 ≤ k ≤ input array's size for non-empty array.

```java
// Assume: k is always valid -- k >= 1 && k <= nums.length;
// Use monotonic queue;
// Observe elements within the window, if a is before b and a <= b, a can no longer
// be the max anymore;
// - use a queue to store the max in window; elements in queue is monotonically
//   decreasing;
// - we push in the queue from right(last) and pop out from left(first);
// - the leftmost (peekFirst) element is the max;
// - for cur, if cur < peekLast: offerLast(cur)
// - if cur >= peekLast: continuesly pollLast until cur < peekLast OR isEmpty, and then
//   offerLast(cur);
// - Thus --> we need a Deque;
// - We also need to pollFirst from the deque if the window has sliding off the element
//   we only need to store [cur, cur_idx+k] into the deque, every time in the iteration
//   check peekFirst, if it's time to pollFirst out;
//
// Time: O(N)

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length < k || k <= 0) {
            return new int[] {};
        }
        int[] res = new int[nums.length - k + 1];
        Deque<int[]> dq = new LinkedList<int[]>();
        for (int i = 0; i < nums.length; i++) {
            if (!dq.isEmpty() && dq.peekFirst()[1] == i) {
                dq.pollFirst();
            }
            if (dq.isEmpty() || nums[i] < dq.peekLast()[0]) {
                dq.offerLast(new int[] {nums[i], i+k});
            }
            else {
                while (!dq.isEmpty() && nums[i] >= dq.peekLast()[0]) {
                    dq.pollLast();
                }
                dq.offerLast(new int[] {nums[i], i+k});
            }
            if (i < k - 1) {
                continue;
            }
            res[i-k+1] = dq.peekFirst()[0];
        }
        return res;
    }
}
```


## 4. Task Scheduler (l.621)
Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks. Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval n that means between two same tasks, there must be at least n intervals that CPU are doing different tasks or just be idle.

You need to return the least number of intervals the CPU will take to finish all the given tasks.

Example:
```
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
```
Note:
The number of tasks is in the range [1, 10000].
The integer n is in the range [0, 100].

```java
// Use idle slots; (draw idle slot table)
// - use map to store tasks and number of task instance (cause we only care number
//   of tasks, we do not need to store task letter actually)
// - sort map based on number of task instances in descending order;
// - max_num = the max number of all tasks' instances;
// - the number of initial slots we have = (max_num-1) * n
// - iterate through all other number of instances in descending order, each time:
//   num_slots -= min(max_num-1, cur_num)
// *Tricky: 
// 1) task names are letters, we can use int map[26] to store the map;
//    cause we don't need task name but only number of instances to calculate, we can 
//   directly Array.sort(map) in O(26 * log(26)) = O(1)
// 2) num_slots can be negative at last --> means we don't need to insert idles, but 
//   letters themselves can intervate each others apart. e.g. A B C D E A  (n = 1)
//
// Time: O(N) --> N number of individual tasks;

class Solution {
    public int leastInterval(char[] tasks, int n) {
        if (tasks == null || tasks.length == 0 || n < 0) {
            return 0;
        }
        int[] map = new int[26];
        for (char c : tasks) {
            map[c - 'A']++;
        }
        Arrays.sort(map);  // ascending order
        int max_value = map[25]-1;
        int num_slots = max_value * n;
        for (int i = 24; i >= 0 && map[i] > 0; i--) { 
            num_slots -= Math.min(max_value, map[i]);
        }
        return num_slots >= 0 ? tasks.length + num_slots : tasks.length;
    }
}
```

## 5. Design Circular Queue (l.622)
Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Your implementation should support following operations:

MyCircularQueue(k): Constructor, set the size of the queue to be k.
Front: Get the front item from the queue. If the queue is empty, return -1.
Rear: Get the last item from the queue. If the queue is empty, return -1.
enQueue(value): Insert an element into the circular queue. Return true if the operation is successful.
deQueue(): Delete an element from the circular queue. Return true if the operation is successful.
isEmpty(): Checks whether the circular queue is empty or not.
isFull(): Checks whether the circular queue is full or not.
 

Example:
```
MyCircularQueue circularQueue = new MyCircularQueue(3); // set the size to be 3
circularQueue.enQueue(1);  // return true
circularQueue.enQueue(2);  // return true
circularQueue.enQueue(3);  // return true
circularQueue.enQueue(4);  // return false, the queue is full
circularQueue.Rear();  // return 3
circularQueue.isFull();  // return true
circularQueue.deQueue();  // return true
circularQueue.enQueue(4);  // return true
circularQueue.Rear();  // return 4
```
Note:

All values will be in the range of [0, 1000].
The number of operations will be in the range of [1, 1000].
Please do not use the built-in Queue library.


```java
// - H points to the first element;
// - T points to the last element;
// - inited: H == T;
// - full: size == a.length;
// - empty: size == 0

class MyCircularQueue {
    protected int[] a;
    protected int h, t, size;

    /** Initialize your data structure here. Set the size of the queue to be k. */
    public MyCircularQueue(int k) {
        if (k <= 0) {
            throw new IllegalArgumentException("size can only be positive");
        }
        this.a = new int[k];
        this.h = 0;
        this.t = 0;
        this.size = 0;
    }
    
    /** Insert an element into the circular queue. Return true if the operation is successful. */
    public boolean enQueue(int value) {
        if (this.isFull()) {
            return false;
        }
        if (this.size > 0) {
            this.t = (this.t + 1) % this.a.length;
        }
        this.a[this.t] = value;
        this.size++;
        return true;
    }
    
    /** Delete an element from the circular queue. Return true if the operation is successful. */
    public boolean deQueue() {
        if (this.isEmpty()) {
            return false;
        }
        if (this.size > 1) {            
            this.h = (this.h + 1) % this.a.length;
        }
        this.size--;
        return true;
    }
    
    /** Get the front item from the queue. */
    public int Front() {
        if (this.isEmpty()) {
            return -1;
        }
        return this.a[this.h];
    }
    
    /** Get the last item from the queue. */
    public int Rear() {
        if (this.isEmpty()) {
            return -1;
        }
        return this.a[this.t];
    }
    
    /** Checks whether the circular queue is empty or not. */
    public boolean isEmpty() {
        return this.size == 0;
    }
    
    /** Checks whether the circular queue is full or not. */
    public boolean isFull() {
        return this.size == a.length;
    }
}

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue obj = new MyCircularQueue(k);
 * boolean param_1 = obj.enQueue(value);
 * boolean param_2 = obj.deQueue();
 * int param_3 = obj.Front();
 * int param_4 = obj.Rear();
 * boolean param_5 = obj.isEmpty();
 * boolean param_6 = obj.isFull();
 */
```