# Graph Strategies

[Link to article](https://leetcode.com/discuss/general-discussion/655708/graph-for-beginners-problems-pattern-sample-solutions/)


## Union Find
 Problems where we combine individual objects into a combined component.

 Keywords : "merge, connection, wired"

```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))
        self.rank = [1] * size

    def find(self, p):
        if self.parent[p] != p:
            self.parent[p] = self.find(self.parent[p])  # Path compression
        return self.parent[p]

    def union(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)

        if rootP != rootQ:
            # Union by rank
            if self.rank[rootP] > self.rank[rootQ]:
                self.parent[rootQ] = rootP
            elif self.rank[rootP] < self.rank[rootQ]:
                self.parent[rootP] = rootQ
            else:
                self.parent[rootQ] = rootP
                self.rank[rootP] += 1

    def connected(self, p, q):
        return self.find(p) == self.find(q)
```
## Depth First Search

### Recursive DFS

```python
def recursive_dfs(graph, node, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(node)
    print(node)  # Process the node, e.g., print it

    for neighbor in graph[node]:
        if neighbor not in visited:
            recursive_dfs(graph, neighbor, visited)

# Example usage
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': ['F'],
    'F': []
}
recursive_dfs(graph, 'A')
```
### Iterative DFS

```python

def iterative_dfs(graph, start):
    visited = set()
    stack = [start]
    
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            print(node)  # Process the node, e.g., print it
            
            # Add neighbors to the stack
            for neighbor in graph[node]:
                if neighbor not in visited:
                    stack.append(neighbor)

# Example usage
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': ['F'],
    'F': []
}

iterative_dfs(graph, 'A')
```
### Start DFS from nodes at boundary:

### Time taken to reach all nodes or share information to all graph nodes:

### DFS from each unvisited node/Island problems

### Cycle Find:

## Depth First Search


## Breadth First Search

```python
from collections import deque

def bfs(graph, start):
    """
    Perform BFS on a graph from a starting node.

    :param graph: A dictionary representing the adjacency list of the graph.
    :param start: The starting node for BFS.
    :return: A list of nodes in the order they were visited.
    """
    visited = set()  # Set to keep track of visited nodes
    queue = deque([start])  # Queue to manage the BFS process
    order = []  # List to record the order of nodes visited

    while queue:
        node = queue.popleft()  # Get the next node from the queue
        if node not in visited:
            visited.add(node)  # Mark the node as visited
            order.append(node)  # Add the node to the visit order

            # Add all unvisited neighbors to the queue
            for neighbor in graph[node]:
                if neighbor not in visited:
                    queue.append(neighbor)

    return order

# Example usage:
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

start_node = 'A'
print(bfs(graph, start_node))  # Output: ['A', 'B', 'C', 'D', 'E', 'F']
```


## Djikstra's Algorithm

```python
import heapq

def dijkstra(graph, start):
    # Initialize distances with infinity and set the distance to the start node to zero
    distances = {vertex: float('infinity') for vertex in graph}
    distances[start] = 0

    # Priority queue to store (distance, vertex) tuples
    priority_queue = [(0, start)]

    while priority_queue:
        current_distance, current_vertex = heapq.heappop(priority_queue)

        # If the current distance is greater than the stored distance, skip processing
        if current_distance > distances[current_vertex]:
            continue

        # Explore neighbors
        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight

            # Only consider this new path if it's better
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(priority_queue, (distance, neighbor))

    return distances

# Example graph represented as an adjacency list
graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'A': 1, 'C': 2, 'D': 5},
    'C': {'A': 4, 'B': 2, 'D': 1},
    'D': {'B': 5, 'C': 1}
}

# Run Dijkstra's algorithm from the source node 'A'
distances = dijkstra(graph, 'A')
print(distances)
```


## Bellman Ford Algorithm

```python
def bellman_ford(graph, start):
    # Initialize distances with infinity and set the distance to the start node to zero
    distances = {vertex: float('infinity') for vertex in graph}
    distances[start] = 0

    # Relax edges up to |V| - 1 times
    for _ in range(len(graph) - 1):
        for u in graph:
            for v, weight in graph[u].items():
                if distances[u] + weight < distances[v]:
                    distances[v] = distances[u] + weight

    # Check for negative weight cycles
    for u in graph:
        for v, weight in graph[u].items():
            if distances[u] + weight < distances[v]:
                raise ValueError("Graph contains a negative weight cycle")

    return distances

# Example graph represented as an adjacency list
graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'C': 2, 'D': 2},
    'C': {'D': 3},
    'D': {'B': -6}
}

# Run Bellman-Ford algorithm from the source node 'A'
try:
    distances = bellman_ford(graph, 'A')
    print(distances)
except ValueError as e:
    print(e)

```
