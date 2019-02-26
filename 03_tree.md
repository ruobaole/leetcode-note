# Tree
---

##1. Binary Tree Preorder Traversal (Iterative) (leetcode 144)
Given a binary tree, return the preorder traversal of its nodes' values.

###Example:
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]

```java
// Use a stack, each time we pop up an element from the stack,
// push it to res; Then push in its right child (first) and left child;

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        if (root == null) {
            return res;
        }
        
        stack.offerFirst(root);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pollFirst();
            res.add(cur.val);
            if (cur.right != null) {
                stack.offerFirst(cur.right);
            }
            if (cur.left != null) {
                stack.offerFirst(cur.left);
            }
        }
        return res;
    }
}

```

---
#2. Binary Tree Postorder Traversal (leetcode 145)
Given a binary tree, return the postorder traversal of its nodes' values.

##Example:
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [3,2,1]

```java
// Traverse with a stack; Each time pop up corrent root from stack;
// We need to know what direction we are coming from:
// 1) from the parent of the cur? --> continue to visit left subtree; --> push into stacl
// 2) from the left child of the cur? --> continue to visit right subtree;
// 3) from the right child of the cur? --> finish all children, visit cur;
// Thus, we need a prev to mark the last node we've visited;
// While loop terminates when -- stack is empty; 

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode prev = null;
        if (root == null) {
            return res;
        }
        
        stack.offerFirst(root);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.peekFirst();
            if (prev == null || prev.left == cur || prev.right == cur) {
                // 1) coming from the parent
                if (cur.left != null) {
                    stack.offerFirst(cur.left);
                }
                else if (cur.right != null) {
                    stack.offerFirst(cur.right);
                }
                else {
                    res.add(cur.val);
                    stack.pollFirst();
                }
            }
            else if (prev == cur.left) {
                // 2) coming from left subtree
                if (cur.right != null) {
                    stack.offerFirst(cur.right);
                }
                else {
                    res.add(cur.val);
                    stack.pollFirst();
                }
            }
            else if (prev == cur.right) {
                // 3) coming from right subtree
                res.add(cur.val);
                stack.pollFirst();
            }
            prev = cur;
        }
        
        return res;
    }
}

```

---
#3. Binary Tree Inorder Traversal (leetcode 94)
Given a binary tree, return the inorder traversal of its nodes' values.

##Example:
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]

```java
// Use a stack to store the cur root;
// Each time we got a current root from the stack, we need to know if we've finished
// the left subtree yet?
// --> we need a helper to mark the next node to visit in cur's subtrees;
// If helper == null -- we know we've finished the left subtree, and we can
// process cur and start iterating the right subtree;
// Else, follow helper to continue;
// While loop terminates only if stack.isEmpty() && helper == null;

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root == null) {
            return res;
        }
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode helper = root;
        while (helper != null || !stack.isEmpty()) {
            if (helper == null) {
                // we've finished the left subtree of cur
                TreeNode cur = stack.pollFirst();
                res.add(cur.val);
                helper = cur.right;
            }
            else {
                // we haven't finish the left subtree
                // continue to follow left subtree
                stack.offerFirst(helper);
                helper = helper.left;
            }
        }
        
        return res;
    }
}

```
---

#4. Balanced Binary Tree (leetcode 110)
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:
a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

##Example 1:
Given the following tree [3,9,20,null,null,15,7]:

    3
   / \
  9  20
    /  \
   15   7
Return true.

##Example 2:
Given the following tree [1,2,2,3,3,null,null,4,4]:

       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
Return false.

```java
// A binary tree is balanced only if its isBalanced(cur.left) && isBalanced(cur.right)
// And Math.abs(height(cur.left) - height(cur.right)) <= 1;
// We can do a post-order DFS;
// isBalancedHelper(cur) returns -1 if tree rooted at cur is not balanced;
// >= 0 --> depth of the subtree;
//
// Time: O(N)

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        return isBalancedHelper(root) != -1;
    }
    
    private int isBalancedHelper(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftDepth = isBalancedHelper(root.left);
        if (leftDepth == -1) {
            return -1;
        }
        int rightDepth = isBalancedHelper(root.right);
        if (rightDepth == -1) {
            return -1;
        }
        if (Math.abs(rightDepth - leftDepth) > 1) {
            return -1;
        }
        
        return Math.max(leftDepth, rightDepth) + 1;
    }
}

```

---
#5. Symmetric Binary Tree (leetcode 101)
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).
For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

    1
   / \
  2   2
 / \ / \
3  4 4  3
But the following [1,2,2,null,3,null,3] is not:
    1
   / \
  2   2
   \   \
   3    3
Note:
**Bonus points if you could solve it both recursively and iteratively**.

```java
// A binary tree is symmetric only if its left subtree is a mirror of its right
// subtree;
// isMirror(one, two): only if one.val == two.val && isMirror(one.left, two.right);
// && isMirror(one.right, two.left);
//
// Iterative way: level-order BFS, push in null if a child doesn't exists; -->
// each level check if current level is symmetric;
// Time: O(N)

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        Deque<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offerLast(root);
        int len = 0;
        while (!queue.isEmpty()) {
            len = queue.size();
            Deque<TreeNode> queue2 = new LinkedList<TreeNode>(queue);
            for (int i = 0; i < len; i++) {
                TreeNode n = queue.pollFirst();
                TreeNode rn = queue2.pollLast();
                if (n != null && rn != null && n.val != rn.val) {
                    return false;
                }
                if ((n == null && rn != null) || (n != null && rn == null)) {
                    return false;
                }
                if (n != null) {
                    queue.offerLast(n.left);
                    queue.offerLast(n.right);                    
                }
            }
        }
        return true;
    }
}

```
---

#6. Treaked Identical Binary Trees
Determine whether two given binary trees are identical assuming any number of ‘tweak’s are allowed. A tweak is defined as a swap of the children of one node in the tree.

##Examples
        5
      /    \
    3        8
  /   \
1      4
and
        5
      /    \
    8        3
           /   \
          1     4
the two binary trees are tweaked identical.

```java
// Two binary trees are treaked-identical only if:
// 1) one.val == two.val
// 2) identical(one.left, two.left) && identical(one.right, two.right) || identical(one.left, two.right) &&
//    identical(one.right, two.left)
// Easy recursive way;
// Iterative way: level-order traverse, each level, check which pairs of children in tree1 has been tweaked,
// according to prev level, compare the tweaked back tree1 level with tree2 level;
// Time: O(N)

public class Solution {
    public boolean isTweakedIdentical(TreeNode one, TreeNode two) {
        if (one == null && two == null) {
            return true;
        }
        if (one == null && two != null || one != null && two == null) {
            return false;
        }
        if (one.val != two.val) {
            return false;
        }

        return isTweakedIdentical(one.left, two.left) && isTweakedIdentical(one.right, two.right) ||
        isTweakedIdentical(one.left, two.right) && isTweakedIdentical(one.right, two.left);
    }
}

```

---
#7. Validate Binary Search Tree (leetcode 98)
Given a binary tree, determine if it is a valid binary search tree (BST).
Assume a BST is defined as follows:
The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

##Example 1:

Input:
    2
   / \
  1   3
Output: true

##Example 2:

    5
   / \
  1   4
     / \
    3   6
Output: false
Explanation: The input is: [5,1,4,null,null,3,6]. The root node's value
             is 5 but its right child's value is 4.

```java
// Assume nodes with duplicate values;
// Pre-order DFS the tree, with a (min, max) bound;
// 1) check if root.val within range (min, max);
// 2) check the left subtree, update max = cur.val-1;
// 3) check the right subtree, update min = cur.val+1;
// initial min, max = null -- means no bound;
//
// Time: O(N)

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBSTHelper(root, null, null);
    }
    
    private boolean isValidBSTHelper(TreeNode root, Integer min, Integer max) {
        if (root == null) {
            return true;
        }
        if (min != null && root.val <= min) {
            return false;
        }
        if (max != null && root.val >= max) {
            return false;
        }

        return isValidBSTHelper(root.left, min, root.val) &&
            isValidBSTHelper(root.right, root.val, max);
    }
}

```

---
#8. Get Keys in BST in a Given Range
Get the list of keys in a given binary search tree in a given range[min, max] in ascending order, both min and max are inclusive.

##Examples
        5
      /    \
    3        8
  /   \        \
 1     4        11
get the keys in [2, 5] in ascending order, result is  [3, 4, 5]

##Corner Cases
* What if there are no keys in the given range? Return an empty list in this case.

```java
// Traverse the tree with a list;
// Left subtree first, then see if current root falls in the list (push in if it is),
// then right subtree;
// If we need the list to be in ascending order -- in-order traversal is needed;
// otherwise we can do pre/post-order;

public class Solution {
    public List<Integer> keysInRange(TreeNode root, int min, int max) {
        List<Integer> res = new ArrayList<Integer>();
        keysInRangeHelper(root, res, min, max);
        return res;
    }

    private void keysInRangeHelper(TreeNode root, List<Integer> res, int min, int max) {
        if (root == null) {
            return;
        }
        keysInRangeHelper(root.left, res, min, max);
        if (root.val <= max && root.val >= min) {
            res.add(root.val);
        }
        keysInRangeHelper(root.right, res, min, max);
    }
}

```

---
#9. Check Completeness of a Binary Tree (leetcode 958)
Given a binary tree, determine if it is a complete binary tree.

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

##Example 1:

Input: [1,2,3,4,5,6]
Output: true
Explanation: Every level before the last is full (ie. levels with node-values {1} and {2, 3}), and all nodes in the last level ({4, 5, 6}) are as far left as possible.

##Example 2:

Input: [1,2,3,4,5,null,7]
Output: false
Explanation: The node with value 7 isn't as far left as possible.
 
##Note:
The tree will have between 1 and 100 nodes.

```java
// BFS the tree with a flag --> true if we've encountered a node with no left/right
// child;
// if flag is true: 1) the node should not have right child;
// 2) all nodes after this node should have no children;
//
// Time: O(N)

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isCompleteTree(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        Deque<TreeNode> queue = new LinkedList<TreeNode>();
        boolean flag = false;
        queue.offerLast(root);
        while (!queue.isEmpty()) {
            TreeNode cur = queue.pollFirst();
            if (cur.left == null) {
                flag = true;
            }
            else if (flag) {
                return false;
            }
            else {
                queue.offerLast(cur.left);
            }
            
            if (cur.right == null) {
                flag = true;
            }
            else if (flag) {
                return false;
            }
            else {
                queue.offerLast(cur.right);
            }
        }
        return true;
    }
}
```

---
#10. Is Graph Bipartite?
Given an undirected graph, return true if and only if it is bipartite.

Recall that a graph is bipartite if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.

The graph is given in the following form: graph[i] is a list of indexes j for which the edge between nodes i and j exists.  Each node is an integer between 0 and graph.length - 1.  There are no self edges or parallel edges: graph[i] does not contain i, and it doesn't contain any element twice.

##Example 1:
Input: [[1,3], [0,2], [1,3], [0,2]]
Output: true
Explanation: 
The graph looks like this:
0----1
|    |
|    |
3----2
We can divide the vertices into two groups: {0, 2} and {1, 3}.

##Example 2:
Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
Output: false
Explanation: 
The graph looks like this:
0----1
| \  |
|  \ |
3----2
We cannot find a way to divide the set of nodes into two independent subsets.
 
##Note:
graph will have length in range [1, 100].
graph[i] will contain integers in range [0, graph.length - 1].
graph[i] will not contain i or duplicate values.
The graph is undirected: if any element j is in graph[i], then i will be in graph[j].

```java
// BFS the graph and trying to partition the graph into 2 groups;
// Mark the original node as group1, for all its neighbors:
// if not visited yet --> mark it has opposite group;
// if visited --> check if it belongs to the opposite group --> false --> 
// not bipartite;
// We use a HashMap<Integer, Integer> to map the idx of graphNode to group number;
//
// *Tricky: for the given list, we cannot be sure if the graph is fully connected.
// Thus, we need to loop the list and check bipartite for each of the element;
// If an element has already been visited, return True directly (we've checked
// it before);
// If not, the given graph if bipartite only of all the individual parts are
// bipartites;
//
// Time: O(N)
// Space: O(N)

class Solution {
    public boolean isBipartite(int[][] graph) {
        if (graph == null) {
            return false;
        }
        
        HashMap<Integer, Integer> visited = new HashMap<Integer, Integer>();
        for (int idx = 0; idx < graph.length; idx++) {
            if (!this.isBipartiteHelper(idx, graph, visited)) {
                return false; 
            }
        }
        return true;
    }
    
    protected boolean isBipartiteHelper(int idx, int[][] graph, HashMap<Integer, Integer> visited) {
        if (visited.containsKey(idx)) {
            // already checked
            return true;
        }
        
        visited.put(idx, 0);
        Deque<Integer> queue = new LinkedList<Integer>();
        queue.offerFirst(idx);
        while (!queue.isEmpty()) {
            int cur = queue.pollLast();
            int neiGroup = visited.get(cur) == 0 ? 1 : 0;
            for (int nei : graph[cur]) {
                if (visited.containsKey(nei)) {
                    if (visited.get(nei) != neiGroup) {
                        return false;
                    }
                }
                else {
                    visited.put(nei, neiGroup);
                    queue.offerFirst(nei);
                }
            }
        }
        return true;
    }
}

```
---

#11. Binary Tree Level Order Traversal (leetcode 102)
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

##For example:
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
return its level order traversal as:
[
  [3],
  [9,20],
  [15,7]
]

```java
// BFS the tree using a queue;
// In the beginning of each layer, keep the length of the queue first
// so that we know we've finish traversing this layer;

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (root == null) {
            return res;
        }
        
        Deque<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offerLast(root);
        while (!queue.isEmpty()) {
            int levelLen = queue.size();
            List<Integer> layer = new ArrayList<Integer>(levelLen);
            for (int i = 0; i < levelLen; i++) {
                TreeNode cur = queue.pollFirst();
                layer.add(cur.val);
                if (cur.left != null) {
                    queue.offerLast(cur.left);
                }
                if (cur.right != null) {
                    queue.offerLast(cur.right);
                }
            }
            res.add(layer);
        }
        return res;
    }
}

```