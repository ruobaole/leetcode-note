# 1. Valid Parenthesis (l. 20)
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

Example 1:
```
Input: "()"
Output: true
```
Example 2:
```
Input: "()[]{}"
Output: true
```
Example 3:
```
Input: "(]"
Output: false
```
Example 4:
```
Input: "([)]"
Output: false
```
Example 5:
```
Input: "{[]}"
Output: true
```

```java
// Assume: s only contains bracket chars
// Use a stack;
// - Everytime we encounter a left bracket --> stack.push(s[i])
// - Everytime we encounter a right bracket --> stack.pop() and see if it matches 
//   current bracket;
// - Check if stack.isEmpty() when s reaches the end;

class Solution {
    public boolean isValid(String s) {
        if (s == null || s.length() == 0) {
            return true;
        }
        Deque<Character> stk = new LinkedList<Character>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(' || s.charAt(i) == '{' || s.charAt(i) == '[') {
                stk.offerFirst(s.charAt(i));
            }
            else if (stk.isEmpty()) {
                return false;
            }
            else if (s.charAt(i) == ')') {
                char tmp = stk.pollFirst();
                if (tmp != '(') {
                    return false;
                }
            }
            else if (s.charAt(i) == '}') {
                char tmp = stk.pollFirst();
                if (tmp != '{') {
                    return false;
                }
            }
            else if (s.charAt(i) == ']') {
                char tmp = stk.pollFirst();
                if (tmp != '[') {
                    return false;
                }
            }
            else {
                return false;
            }
        }
        return stk.isEmpty();
    }
}
```

# 2. Remove k Digits (l.402)
Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.

Note:
The length of num is less than 10002 and will be ≥ k.
The given num does not contain any leading zero.
Example 1:
```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
```
Example 2:
```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
```
Example 3:
```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
```

```java
// Assume: if k >= num.length() --> return "0";
// Greedy and Use Stack;
// - It is just finding the k local maximas and delete them
// - Iterate from left->right, pushing chars into stack;
// - If we encounter a descendent (num[i] < stack.top()) -- stack.top() is the local
//   maxima --> we should pop from stack;
// - If num is in non-decreasing order, we just need to delete k chars from tail;
// - Delete all leading 0s;
// *Careful -- if stack is empty --> return '0' at last;
//
// Time: O(N)
// Space: O(N) -- use the StringBuilder as stack

class Solution {
    public String removeKdigits(String num, int k) {
        if (num == null || num.length() == 0 || num.length() <= k) {
            return "0";
        }
        if (k <= 0) {
            return num;
        }
        StringBuilder sb = new StringBuilder();
        sb.append(num.charAt(0));
        int cnt = 0;
        for (int i = 1; i < num.length(); i++) {
            if (sb.length() > 0 && cnt < k && num.charAt(i) < sb.charAt(sb.length()-1)) {
                sb.deleteCharAt(sb.length()-1);
                cnt++;
                i--;
            }
            else {
                sb.append(num.charAt(i));
            }
        }
        // remove from tail if non-decreasing
        while (sb.length() > 0 && cnt < k) {
            sb.deleteCharAt(sb.length()-1);
            cnt++;
        }
        // remove leading 0s
        while (sb.length() > 0 && sb.charAt(0) == '0') {
            sb.deleteCharAt(0);
        }
        return sb.length() == 0 ? "0" : sb.toString();
    }
}
```

# 3. Validate Stack Sequences (l.946)
Given two sequences pushed and popped with distinct values, return true if and only if this could have been the result of a sequence of push and pop operations on an initially empty stack.

Example 1:
```
Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
Output: true
Explanation: We might do the following sequence:
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```
Example 2:
```
Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
Output: false
Explanation: 1 cannot be popped before 2.
```
Note:
```
0 <= pushed.length == popped.length <= 1000
0 <= pushed[i], popped[i] < 1000
pushed is a permutation of popped.
pushed and popped have distinct values.
```

```java
// Assume: pushed.length == popped.length && pushed is a permutation of popped;
// Greedy and Stack;
// - Iterate popped left->right;
// - If stack.top() != popped[i]: push values to stack until we got stack.top()
//   == popped[i]; --> If we reached the end of pushed[] --> false;
// - If stack.top() == popped[i]: pop stack, and make i++;
// - If we've reached the end of popped and stack.isEmpty() --> true;
//
// Time: O(N)
// Space: O(N)

class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        if (pushed == null || popped == null || pushed.length != popped.length) {
            return false;
        }
        if (pushed.length == 0) {
            return true;
        }
        Deque<Integer> stk = new LinkedList<Integer>();
        stk.offerFirst(pushed[0]);
        int pu = 1, po = 0;
        while (po < popped.length) {
            while (po < popped.length && !stk.isEmpty()
                   && stk.peekFirst() == popped[po])
            {
                stk.pollFirst();
                po++;
            }
            if (po == popped.length && !stk.isEmpty()) {
                return false;
            }
            while (pu < pushed.length &&
                   (stk.isEmpty() || stk.peekFirst() != popped[po]))
            {
                stk.offerFirst(pushed[pu++]);
            }
            if (pu == pushed.length && !stk.isEmpty()
                && stk.peekFirst() != popped[po]) {
                return false;
            }
        }
        if (po == popped.length && pu == pushed.length && stk.isEmpty()) {
            return true;
        }
        return false;
    }
}
```