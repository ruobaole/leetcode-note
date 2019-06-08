# Graph
## Clone Undirectional Graph (l. 133)
Given a reference of a node in a connected undirected graph, return a deep copy (clone) of the graph. Each node in the graph contains a val (int) and a list (List[Node]) of its neighbors.

Example:
```
Input:
{"$id":"1","neighbors":[{"$id":"2","neighbors":[{"$ref":"1"},{"$id":"3","neighbors":[{"$ref":"2"},{"$id":"4","neighbors":[{"$ref":"3"},{"$ref":"1"}],"val":4}],"val":3}],"val":2},{"$ref":"4"}],"val":1}
```
Explanation:
Node 1's value is 1, and it has two neighbors: Node 2 and 4.
Node 2's value is 2, and it has two neighbors: Node 1 and 3.
Node 3's value is 3, and it has two neighbors: Node 2 and 4.
Node 4's value is 4, and it has two neighbors: Node 1 and 3.


Note:
The number of nodes will be between 1 and 100.
The undirected graph is a simple graph, which means no repeated edges and no self-loops in the graph.
Since the graph is undirected, if node p has node q as neighbor, then node q must have node p as neighbor too.
You must return the copy of the given node as a reference to the cloned graph.

```java
// BFS the graph, check if we've visited the node;
// - Besides, even if a node is visited, we also need to adds it
//   as the neighbor of the current node;
// - Thus, using a Map<Node, Node> to map node -> clonedNode, also
//   check if visited;
// - For each node u -> iterate all its neighbors v -->
//   get clonedV, if clonedV == null --> created one, 
//   adds to u.neighbor, adds to queue
//
// Time: O(V+E)

/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;

    public Node() {}

    public Node(int _val,List<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return null;
        }
        Deque<Node> queue = new LinkedList<Node>();
        Map<Node, Node> map = new HashMap<Node, Node>();
        queue.addLast(node);
        map.put(node, new Node(node.val, new ArrayList<Node>() ));
        
        while (!queue.isEmpty()) {
            Node n = queue.pollFirst();
            Node clonedN = map.get(n);
            if (n.neighbors != null) {
                for (Node nei : n.neighbors) {
                    if (map.get(nei) == null) {
                        Node clonedNei = new Node(nei.val, new ArrayList<Node>());
                        clonedN.neighbors.add(clonedNei);
                        map.put(nei, clonedNei);
                        queue.offerLast(nei);
                    }
                    else {
                        Node clonedNei = map.get(nei);
                        clonedN.neighbors.add(clonedNei);
                    }
                }
            }
        }
        return map.get(node);
    }
}
```