# Graph Traversal

### DFS (depth-first search)

DFS uses recursion to visit each child node to its full depth before moving on to the next. It requires a mechanism to mark nodes as visited (maybe a property on the nodes, or a hashset).

### BFS (breadth-first search)

BFS uses a queue and a `while` loop to visit each child node layer by layer. All child nodes are added to the queue to be visited, and their children are added to the queue. The queue's FIFO property means that "deeper" nodes won't be checked before more shallow nodes.

### Dijkstra's algorithm

Dijkstra's algorithm finds the shortest path between nodes in a graph. Given a graph with `V` nodes and a specific starting node, the algorithm is:

1. Create a hashset that keeps tracked of visited nodes (initially empty)
2. Create a hashmap/array of distance values to all nodes from the starting node, initialize all to infinity (`Number.MAX_VALUE`)
3. While the hashset doesn't include all nodes:
    1. Pick a node `u` that isn't in the set and has a minimum distance value from any known node
    2. Include it in the set (mark it as seen)
    3. Update the distance value for all nodes reachable from `u`:
        * Iterate through all adjacent nodes
        * For each node `v`, if the sum of the distance from the starting node to `u` and the distance from `u` to `v` is less than the current distance value of `v` (infinite if an undiscovered node), then update its distance value

Runtime complexity: O(V^2)

https://www.geeksforgeeks.org/dijkstras-shortest-path-algorithm-greedy-algo-7/

---

