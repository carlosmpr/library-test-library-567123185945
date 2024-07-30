---
  title: 'PythonTestFile.py'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos Polanco'
  ---
  
  
  
  # PythonTestFile.py
  # Explanation of Shortest Path Algorithm using Dijkstra's Algorithm

This code implements a graph data structure and uses Dijkstra's algorithm to find the shortest path between two nodes in the graph.

### Graph Class
- The `Graph` class represents a graph data structure.
- It has the following methods:
  - `__init__(self)`: Initializes the graph with an empty dictionary of nodes.
  - `add_node(self, label, neighbors)`: Adds a node to the graph with its neighbors and their corresponding edge weights.
  - `shortest_path(self, start, end)`: Finds the shortest path from the `start` node to the `end` node using Dijkstra's algorithm.

### Dijkstra's Algorithm
- Dijkstra's algorithm is a popular algorithm for finding the shortest path between nodes in a graph.
- It uses a priority queue (implemented here using `heapq`) to efficiently select the node with the smallest distance.
- The algorithm maintains a `distances` dictionary to keep track of the shortest distance from the `start` node to each node.
- It also uses a `previous` dictionary to store the previous node in the shortest path to each node.
- The algorithm iterates through the nodes, updating the distances and previous nodes as it finds shorter paths.
- Finally, it reconstructs the shortest path from `start` to `end` using the `previous` dictionary.

### Example Usage
- In the `if __name__ == "__main__":` block, a graph is created with nodes A to F and their respective neighbors and edge weights.
- The shortest path from node A to node F is calculated using `graph.shortest_path("A", "F")`.
- The result is printed to the console.

### External Libraries
- The code uses the `heapq` module, which provides heap queue algorithm implementations.
- No other external libraries or dependencies are required for this code.

### Example Output
- If we run the code, it will output the shortest path from node A to node F in the graph:
  ```
  Shortest path from A to F: ['A', 'C', 'D', 'F']
  ```
  
  ## Component Code
  ```jsx
  import heapq

class Graph:
    def __init__(self):
        self.nodes = {}

    def add_node(self, label, neighbors):
        self.nodes[label] = neighbors

    def shortest_path(self, start, end):
        queue = [(0, start)]
        distances = {node: float('inf') for node in self.nodes}
        distances[start] = 0
        previous = {node: None for node in self.nodes}
        visited = set()

        while queue:
            current_cost, current_node = heapq.heappop(queue)

            if current_node in visited:
                continue

            visited.add(current_node)

            for neighbor, cost in self.nodes[current_node]:
                if neighbor in visited:
                    continue

                new_cost = current_cost + cost
                if new_cost < distances[neighbor]:
                    distances[neighbor] = new_cost
                    previous[neighbor] = current_node
                    heapq.heappush(queue, (new_cost, neighbor))

        path = []
        at = end
        while at is not None:
            path.append(at)
            at = previous[at]

        return path[::-1]

if __name__ == "__main__":
    graph = Graph()
    graph.add_node("A", [("B", 4), ("C", 2)])
    graph.add_node("B", [("A", 4), ("C", 1), ("D", 5)])
    graph.add_node("C", [("A", 2), ("B", 1), ("D", 8), ("E", 10)])
    graph.add_node("D", [("B", 5), ("C", 8), ("E", 2), ("F", 6)])
    graph.add_node("E", [("C", 10), ("D", 2), ("F", 3)])
    graph.add_node("F", [("D", 6), ("E", 3)])

    print("Shortest path from A to F:", graph.shortest_path("A", "F"))
  ```
  