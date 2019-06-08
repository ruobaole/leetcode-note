# Graph
https://www.geeksforgeeks.org/graph-data-structure-and-algorithms/
## Graph Representation
- Ajacency matrix
- Ajacency list

## Graph Traversals
### DFS
- Same as DFS tree, except for using a ```boolean[] visited``` to mark for visited nodes;
- For **disconnected graph**, we have to DFS all vertices to make sure all vertices is visited
- https://www.geeksforgeeks.org/applications-of-depth-first-search/
- Time: O(V + E)
### BFS
- Same as DFS tree, except for using a ```boolean[] visited``` to mark for visited nodes;
- https://www.geeksforgeeks.org/applications-of-breadth-first-traversal/
- Time: O(V + E)

## Graph Cycle
### Cycle Detect -- (back edge)
- **Directional** Graph -- DFS with a recursion Stack; Check if a vertix is already in recStack; --> forming a back edge with the current ver;
- **Undirectional** Graph (Assumes no self-loop) -- [**Union-Find (Disjoint-set)**](https://www.geeksforgeeks.org/union-find/).
    - Naive way -- O(E*V)
    - [Union by Rank or Height](https://www.geeksforgeeks.org/union-find-algorithm-set-2-union-by-rank/) --> O(ElogV)
- **Undirectional** Graph (Assumes no self-loop) -- [DFS](https://www.geeksforgeeks.org/detect-cycle-undirected-graph/) -- for every visited vertix 'v', if there're any adjacent 'u' such that u is visited and u is not parent of v --> cycle!

## Minimum Spanning Tree  --> GREEDY
- For **connected** **undirectional** graph: a spanning tree with weight less than or equal to the weight of every other spanning tree;
- [Kruskal](ttps://www.geeksforgeeks.org/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/) (use Union-Find) -- O(ElogE + ElogV)
- Prim -- O(V^2)

## Shortest Path
- [Dijkstra](https://www.geeksforgeeks.org/dijkstras-shortest-path-algorithm-greedy-algo-7/)
    - for both **directional** and **undirectional** graphs;
    - O(N^2)
    - doesn't work for graph with negative weight --> Bellman-Ford instead;

## Back Tracking
- n-queen's problem