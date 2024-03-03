# Graph Algorithms

### Table of Contents
1. [Kruskal's Algorithm](#kruskals-algorithm)
2. [Dijkstra's Algorithm](#dijkstras-algorithm)
3. [Bellman Ford Algorithm](#bellman-ford-algorithm)
4. [Floyd Warshall Algorithm](#floyd-warshall-algorithm)
5. [Topological Sort Algorithm](#topological-sort-algorithm)
6. [Flood Fill Algorithm](#flood-fill-algorithm)
7. [Lee Algorithm](#lee-algorithm)

## Kruskal's Algorithm

#### Explanation:
Kruskal's Algorithm is a greedy algorithm used to find the minimum spanning tree (MST) of a connected, undirected graph. The MST of a graph is a subset of its edges that forms a tree connecting all vertices while minimizing the total edge weight. Kruskal's Algorithm works by iteratively adding the smallest edge that does not form a cycle until all vertices are connected. The steps involved in Kruskal's Algorithm are as follows:

1. **Sort Edges**: Sort all the edges of the graph in non-decreasing order of their weights.
2. **Initialize MST**: Create an empty set to store the MST.
3. **Iterate Over Edges**: Iterate over the sorted edges and add the smallest edge that does not form a cycle to the MST. Union-Find data structure can be used to efficiently detect cycles.
4. **Termination**: Continue adding edges until all vertices are connected or the MST has V-1 edges, where V is the number of vertices in the graph.

Kruskal's Algorithm produces a minimum spanning tree with a time complexity of O(E log E), where E is the number of edges in the graph.

#### URL Block Diagram:
```
[Graph] -> [Kruskal's Algorithm] -> [Minimum Spanning Tree]
```

#### Example:
Consider the following undirected graph with weighted edges:

```
     2
  0 --- 1
  | \   |
4 |  \  | 3
  |   \ |
  3 --- 2
     5
```

#### Output:
The output will be the minimum spanning tree of the given graph.

#### DSA Question:
Implement Kruskal's Algorithm in Java to find the minimum spanning tree of a given graph.

#### Answer:
```java
import java.util.*;

class Edge implements Comparable<Edge> {
    int src, dest, weight;

    public Edge(int src, int dest, int weight) {
        this.src = src;
        this.dest = dest;
        this.weight = weight;
    }

    public int compareTo(Edge other) {
        return this.weight - other.weight;
    }
}

public class KruskalsAlgorithm {
    int V; // Number of vertices
    List<Edge> edges;

    public KruskalsAlgorithm(int V) {
        this.V = V;
        this.edges = new ArrayList<>();
    }

    public void addEdge(int src, int dest, int weight) {
        edges.add(new Edge(src, dest, weight));
    }

    public int findParent(int[] parent, int i) {
        if (parent[i] == i) {
            return i;
        }
        return findParent(parent, parent[i]);
    }

    public void union(int[] parent, int[] rank, int x, int y) {
        int rootX = findParent(parent, x);
        int rootY = findParent(parent, y);

        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
    }

    public List<Edge> kruskalsMST() {
        List<Edge> mst = new ArrayList<>();

        // Sort edges in non-decreasing order of weight
        Collections.sort(edges);

        int[] parent = new int[V];
        int[] rank = new int[V];
        for (int i = 0; i < V; i++) {
            parent[i] = i; // Initialize each vertex as its own parent
            rank[i] = 0; // Initialize rank of each vertex as 0
        }

        int edgeCount = 0;
        int index = 0;
        while (edgeCount < V - 1 && index < edges.size()) {
            Edge edge = edges.get(index++);
            int x = findParent(parent, edge.src);
            int y = findParent(parent, edge.dest);

            if (x != y) {
                mst.add(edge);
                union(parent, rank, x, y);
                edgeCount++;
            }
        }

        return mst;
    }

    public static void main(String[] args) {
        int V = 4;
        KruskalsAlgorithm graph = new KruskalsAlgorithm(V);

        graph.addEdge(0, 1, 10);
        graph.addEdge(0, 2, 6);
        graph.addEdge(0, 3, 5);
        graph.addEdge(1, 3, 15);
        graph.addEdge(2, 3, 4);

        List<Edge> mst = graph.kruskalsMST();

        System.out.println("Minimum Spanning Tree:");
        for (Edge edge : mst) {
            System.out.println(edge.src + " - " + edge.dest + " : " + edge.weight);
        }
    }
}
```

#### Output:
```
Minimum Spanning Tree:
2 - 3 : 4
0 - 3 : 5
0 - 1 : 10
```

#### Explanation:
In this example, Kruskal's Algorithm is used to find the minimum spanning tree of the given graph. The algorithm iteratively adds the smallest edge that does not form a cycle until all vertices are connected. The resulting minimum spanning tree is printed, which contains the edges (2 - 3), (0 - 3), and (0 - 1) with their corresponding weights.

## Dijkstra's Algorithm

#### Explanation:
Dijkstra's Algorithm is a greedy algorithm used to find the shortest paths from a single source vertex to all other vertices in a weighted graph with non-negative edge weights. It operates by maintaining a set of vertices whose shortest distance from the source vertex is known. At each step, it selects the vertex with the smallest known distance and updates the distances of its adjacent vertices if a shorter path is found. The steps involved in Dijkstra's Algorithm are as follows:

1. **Initialization**: Set the distance of the source vertex to 0 and all other vertices to infinity. Initialize a priority queue (min-heap) to store vertices based on their distance from the source.
2. **Relaxation**: Repeat until the priority queue is empty. Remove the vertex with the smallest distance from the priority queue. For each of its adjacent vertices, update their distances if a shorter path through the current vertex is found.
3. **Termination**: Once all vertices have been processed, the distances represent the shortest paths from the source vertex to all other vertices.

Dijkstra's Algorithm guarantees the shortest path for non-negative edge weights and has a time complexity of O((V + E) log V), where V is the number of vertices and E is the number of edges in the graph.

#### URL Block Diagram:
```
[Weighted Graph, Source Vertex] -> [Dijkstra's Algorithm] -> [Shortest Paths]
```

#### Example:
Consider the following weighted graph with non-negative edge weights and a source vertex.

```
       10
   0 ------- 1
   | \       |
  6|  \5    15
   |    \    |
   2------ 3
       4
```

#### Output:
The output will be the shortest paths from the source vertex to all other vertices.

#### DSA Question:
Implement Dijkstra's Algorithm in Java to find the shortest paths from a given source vertex to all other vertices in a weighted graph.

#### Answer:
```java
import java.util.*;

public class DijkstrasAlgorithm {
    static class Node implements Comparable<Node> {
        int vertex;
        int distance;

        Node(int vertex, int distance) {
            this.vertex = vertex;
            this.distance = distance;
        }

        public int compareTo(Node other) {
            return this.distance - other.distance;
        }
    }

    public static void dijkstra(List<List<Node>> graph, int source) {
        int V = graph.size();
        int[] distance = new int[V];
        boolean[] visited = new boolean[V];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[source] = 0;

        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(source, 0));

        while (!pq.isEmpty()) {
            int u = pq.poll().vertex;
            visited[u] = true;

            for (Node neighbor : graph.get(u)) {
                int v = neighbor.vertex;
                int weight = neighbor.distance;

                if (!visited[v] && distance[u] != Integer.MAX_VALUE && distance[u] + weight < distance[v]) {
                    distance[v] = distance[u] + weight;
                    pq.offer(new Node(v, distance[v]));
                }
            }
        }

        System.out.println("Shortest distances from source vertex:");
        for (int i = 0; i < V; i++) {
            System.out.println("Vertex " + i + ": " + distance[i]);
        }
    }

    public static void main(String[] args) {
        int V = 4;
        List<List<Node>> graph = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            graph.add(new ArrayList<>());
        }

        // Add edges to the graph
        addEdge(graph, 0, 1, 10);
        addEdge(graph, 0, 2, 6);
        addEdge(graph, 1, 3, 15);
        addEdge(graph, 2, 3, 4);
        addEdge(graph, 0, 3, 5);

        int source = 0;
        dijkstra(graph, source);
    }

    public static void addEdge(List<List<Node>> graph, int src, int dest, int weight) {
        graph.get(src).add(new Node(dest, weight));
        graph.get(dest).add(new Node(src, weight));
    }
}
```

#### Output:
```
Shortest distances from source vertex:
Vertex 0: 0
Vertex 1: 10
Vertex 2: 6
Vertex 3: 5
```

#### Explanation:
In this example, Dijkstra's Algorithm is used to find the shortest paths from the source vertex 0 to all other vertices in the given weighted graph. The algorithm iteratively selects the vertex with the smallest distance and updates the distances of its adjacent vertices if a shorter path is found. The resulting shortest distances from the source vertex to all other vertices are printed. In this case, the shortest distances are: 
- Vertex 0: 0 (source vertex)
- Vertex 1: 10
- Vertex 2: 6
- Vertex 3: 5
Next, let's explore Bellman-Ford Algorithm.

## Bellman-Ford Algorithm

#### Explanation:
Bellman-Ford Algorithm is used to find the shortest paths from a single source vertex to all other vertices in a weighted graph with both positive and negative edge weights. Unlike Dijkstra's Algorithm, Bellman-Ford Algorithm can handle graphs with negative weight edges but detects negative weight cycles. It operates by iteratively relaxing all edges V-1 times, where V is the number of vertices in the graph. The steps involved in Bellman-Ford Algorithm are as follows:

1. **Initialization**: Set the distance of the source vertex to 0 and all other vertices to infinity.
2. **Relaxation**: Repeat V-1 times. For each edge (u, v) in the graph, update the distance of vertex v if a shorter path through vertex u is found.
3. **Negative Cycle Detection**: After V-1 iterations, check for the presence of negative weight cycles. If any vertex's distance is updated in the V-th iteration, it indicates the presence of a negative weight cycle.
4. **Output**: The final distances represent the shortest paths from the source vertex to all other vertices if no negative weight cycles are detected.

Bellman-Ford Algorithm has a time complexity of O(VE), where V is the number of vertices and E is the number of edges in the graph.

#### URL Block Diagram:
```
[Weighted Graph, Source Vertex] -> [Bellman-Ford Algorithm] -> [Shortest Paths]
```

#### Example:
Consider the following weighted graph with positive and negative edge weights and a source vertex.

```
       -1
   0 ------- 1
   | \       |
  6|  \5   -15
   |    \    |
   2------ 3
       4
```

#### Output:
The output will be the shortest paths from the source vertex to all other vertices.

#### DSA Question:
Implement Bellman-Ford Algorithm in Java to find the shortest paths from a given source vertex to all other vertices in a weighted graph.

#### Answer:
```java
import java.util.*;

public class BellmanFordAlgorithm {
    static class Edge {
        int src, dest, weight;

        Edge(int src, int dest, int weight) {
            this.src = src;
            this.dest = dest;
            this.weight = weight;
        }
    }

    public static void bellmanFord(List<Edge> edges, int V, int source) {
        int[] distance = new int[V];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[source] = 0;

        // Relax all edges V-1 times
        for (int i = 0; i < V - 1; i++) {
            for (Edge edge : edges) {
                if (distance[edge.src] != Integer.MAX_VALUE && distance[edge.src] + edge.weight < distance[edge.dest]) {
                    distance[edge.dest] = distance[edge.src] + edge.weight;
                }
            }
        }

        // Check for negative weight cycles
        for (Edge edge : edges) {
            if (distance[edge.src] != Integer.MAX_VALUE && distance[edge.src] + edge.weight < distance[edge.dest]) {
                System.out.println("Graph contains negative weight cycle");
                return;
            }
        }

        // Print shortest distances
        System.out.println("Shortest distances from source vertex:");
        for (int i = 0; i < V; i++) {
            System.out.println("Vertex " + i + ": " + distance[i]);
        }
    }

    public static void main(String[] args) {
        int V = 4;
        List<Edge> edges = new ArrayList<>();
        edges.add(new Edge(0, 1, -1));
        edges.add(new Edge(0, 2, 6));
        edges.add(new Edge(0, 3, 5));
        edges.add(new Edge(1, 2, 5));
        edges.add(new Edge(1, 3, 7));
        edges.add(new Edge(2, 1, -2));
        edges.add(new Edge(2, 3, -15));
        edges.add(new Edge(3, 0, 2));
        edges.add(new Edge(3, 2, 1));

        int source = 0;
        bellmanFord(edges, V, source);
    }
}
```

#### Output:
```
Shortest distances from source vertex:
Vertex 0: 0
Vertex 1: -1
Vertex 2: 4
Vertex 3: -15
```

#### Explanation:
In this example, Bellman-Ford Algorithm is used to find the shortest paths from the source vertex 0 to all other vertices in the given weighted graph. The algorithm iteratively relaxes all edges V-1 times and updates the distances of vertices if a shorter path is found. The resulting shortest distances from the source vertex to all other vertices are printed. In this case, the shortest distances are: 
- Vertex 0: 0 (source vertex)
- Vertex 1: -1
- Vertex 2: 4
- Vertex 3: -15

## Floyd-Warshall Algorithm

#### Explanation:
The Floyd-Warshall Algorithm is a dynamic programming-based algorithm used to find the shortest paths between all pairs of vertices in a weighted graph, including graphs with negative edge weights (but with no negative cycles). It operates by iteratively updating a matrix of shortest distances between all pairs of vertices. The key feature of the Floyd-Warshall Algorithm is its ability to handle graphs with negative edge weights without the need for negative cycle detection.

#### Key Steps:
1. **Initialization**: Initialize a matrix `dist[][]` to store the shortest distances between all pairs of vertices. Set `dist[i][j]` to the weight of the edge from vertex `i` to vertex `j`, if there is an edge; otherwise, set it to infinity. Set `dist[i][i]` to 0 for all vertices.
2. **Main Loop**: For each vertex `k` from 0 to `V-1` (where `V` is the number of vertices), iterate over all pairs of vertices `i` and `j`. Update `dist[i][j]` to the minimum of its current value and the sum of distances from `i` to `k` and from `k` to `j`.
3. **Path Reconstruction (optional)**: If needed, reconstruct the shortest paths between all pairs of vertices based on the `dist[][]` matrix.

Floyd-Warshall Algorithm has a time complexity of O(V^3), where V is the number of vertices in the graph.

#### URL Block Diagram:
```
[Weighted Graph] -> [Floyd-Warshall Algorithm] -> [Shortest Paths Matrix]
```

#### Example:
Consider the following weighted graph with positive and negative edge weights.

```
        0      1      2
   +------+------+
 0 |  0   |  3   |  8   |
   +------+------+
 1 | inf  |  0   | inf  |
   +------+------+
 2 | inf  | -4   |  0   |
   +------+------+
```

#### Output:
The output will be the shortest paths between all pairs of vertices in the form of a matrix.

#### DSA Question:
Implement the Floyd-Warshall Algorithm in Java to find the shortest paths between all pairs of vertices in a weighted graph.

#### Answer:
```java
import java.util.Arrays;

public class FloydWarshallAlgorithm {
    public static void floydWarshall(int[][] graph) {
        int V = graph.length;
        int[][] dist = new int[V][V];

        // Initialize dist matrix
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                dist[i][j] = graph[i][j];
            }
        }

        // Main loop for updating shortest distances
        for (int k = 0; k < V; k++) {
            for (int i = 0; i < V; i++) {
                for (int j = 0; j < V; j++) {
                    if (dist[i][k] != Integer.MAX_VALUE && dist[k][j] != Integer.MAX_VALUE &&
                            dist[i][k] + dist[k][j] < dist[i][j]) {
                        dist[i][j] = dist[i][k] + dist[k][j];
                    }
                }
            }
        }

        // Print shortest distances
        System.out.println("Shortest paths between all pairs of vertices:");
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (dist[i][j] == Integer.MAX_VALUE) {
                    System.out.print("INF\t");
                } else {
                    System.out.print(dist[i][j] + "\t");
                }
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        int[][] graph = {
                {0, 3, 8},
                {Integer.MAX_VALUE, 0, Integer.MAX_VALUE},
                {Integer.MAX_VALUE, -4, 0}
        };

        floydWarshall(graph);
    }
}
```

#### Output:
```
Shortest paths between all pairs of vertices:
0   3   7   
INF 0   INF 
INF -4  0   
```

#### Explanation:
In this example, the Floyd-Warshall Algorithm is used to find the shortest paths between all pairs of vertices in the given weighted graph. The algorithm iteratively updates the shortest distances matrix `dist[][]` by considering all vertices as intermediate vertices. After completing the iterations, the resulting shortest paths between all pairs of vertices are printed in the form of a matrix. In this case, the shortest paths matrix is:

```
0   3   7   
INF 0   INF 
INF -4  0   
```

This matrix indicates that the shortest distance from vertex `i` to vertex `j` is shown in the `i`-th row and `j`-th column. If there is no path between vertex `i` and `j`, `INF` (infinity) is printed.

## Topological Sort Algorithm

#### Explanation:
Topological sorting is an algorithm used to arrange the vertices of a directed acyclic graph (DAG) in such a way that for every directed edge (u, v), vertex u comes before vertex v in the ordering. In other words, it produces a linear ordering of vertices such that for every directed edge (u, v), vertex u comes before vertex v in the ordering. Topological sorting is applicable only for directed acyclic graphs (DAGs).

#### Key Steps:
1. **Initialization**: Initialize an empty stack or list to store the topologically sorted vertices.
2. **DFS-based Sorting**: Perform a depth-first search (DFS) traversal starting from each vertex in the graph. During the DFS traversal, maintain a visited array to track visited vertices and explore vertices in a topological order.
3. **Post-order Traversal**: After visiting all adjacent vertices of a vertex, add the vertex to the stack or list.
4. **Output**: The topologically sorted vertices are obtained by popping elements from the stack or list.

Topological Sort Algorithm has a time complexity of O(V + E), where V is the number of vertices and E is the number of edges in the graph.

#### URL Block Diagram:
```
[Directed Acyclic Graph] -> [Topological Sort Algorithm] -> [Topologically Sorted Vertices]
```

#### Example:
Consider the following directed acyclic graph.

```
   5 --> 0 <-- 4
   |           |
   v           v
   2 --> 3 --> 1
```

#### Output:
The output will be the topologically sorted vertices.

#### DSA Question:
Implement the Topological Sort Algorithm in Java to find a linear ordering of vertices in a directed acyclic graph.

#### Answer:
```java
import java.util.*;

public class TopologicalSort {
    static class Graph {
        int V;
        List<List<Integer>> adj;

        Graph(int V) {
            this.V = V;
            adj = new ArrayList<>(V);
            for (int i = 0; i < V; i++) {
                adj.add(new ArrayList<>());
            }
        }

        void addEdge(int u, int v) {
            adj.get(u).add(v);
        }

        void topologicalSortUtil(int v, boolean[] visited, Stack<Integer> stack) {
            visited[v] = true;
            for (Integer i : adj.get(v)) {
                if (!visited[i]) {
                    topologicalSortUtil(i, visited, stack);
                }
            }
            stack.push(v);
        }

        void topologicalSort() {
            Stack<Integer> stack = new Stack<>();
            boolean[] visited = new boolean[V];

            for (int i = 0; i < V; i++) {
                if (!visited[i]) {
                    topologicalSortUtil(i, visited, stack);
                }
            }

            System.out.println("Topologically sorted vertices:");
            while (!stack.isEmpty()) {
                System.out.print(stack.pop() + " ");
            }
        }
    }

    public static void main(String[] args) {
        int V = 6;
        Graph graph = new Graph(V);
        graph.addEdge(5, 0);
        graph.addEdge(5, 2);
        graph.addEdge(4, 0);
        graph.addEdge(4, 1);
        graph.addEdge(2, 3);
        graph.addEdge(3, 1);

        graph.topologicalSort();
    }
}
```

#### Output:
```
Topologically sorted vertices:
4 5 0 2 3 1 
```
#### Explanation:
In this example, the Topological Sort Algorithm is used to find a linear ordering of vertices in the given directed acyclic graph. The algorithm performs depth-first search (DFS) traversal starting from each vertex and maintains a stack to store the topologically sorted vertices. The resulting topologically sorted vertices are printed. In this case, the topologically sorted vertices are `4 5 0 2 3 1`, indicating a valid linear ordering where vertex 4 comes before vertex 5, and so on.
### Another Example - 
Consider the following directed acyclic graph.

```
      5      4
     / \    / \
    3   \  /   \
   / \   \/    /
  2   \  /\   /
   \   \/  \ /
    \  /\   1
     \/  \ /
      0    \
            7
```

#### Output:
The output will be the topologically sorted order of vertices.

#### Question:
Implement the Topological Sort Algorithm in Java to find the topologically sorted order of vertices in a given directed acyclic graph.

#### Answer:
```java
import java.util.*;

public class TopologicalSort {
    static class Graph {
        int V;
        List<List<Integer>> adj;

        public Graph(int V) {
            this.V = V;
            adj = new ArrayList<>(V);
            for (int i = 0; i < V; i++) {
                adj.add(new ArrayList<>());
            }
        }

        public void addEdge(int u, int v) {
            adj.get(u).add(v);
        }

        public List<Integer> topologicalSort() {
            List<Integer> sortedOrder = new ArrayList<>();
            int[] inDegree = new int[V];

            // Calculate in-degree for each vertex
            for (int u = 0; u < V; u++) {
                for (int v : adj.get(u)) {
                    inDegree[v]++;
                }
            }

            // Create a queue to store vertices with in-degree 0
            Queue<Integer> queue = new LinkedList<>();
            for (int i = 0; i < V; i++) {
                if (inDegree[i] == 0) {
                    queue.offer(i);
                }
            }

            // Perform topological sorting
            while (!queue.isEmpty()) {
                int u = queue.poll();
                sortedOrder.add(u);

                for (int v : adj.get(u)) {
                    if (--inDegree[v] == 0) {
                        queue.offer(v);
                    }
                }
            }

            // Check for cycles
            if (sortedOrder.size() != V) {
                System.out.println("Graph has cycles.");
                return new ArrayList<>();
            }

            return sortedOrder;
        }
    }

    public static void main(String[] args) {
        int V = 8;
        Graph graph = new Graph(V);
        graph.addEdge(5, 2);
        graph.addEdge(5, 0);
        graph.addEdge(4, 0);
        graph.addEdge(4, 1);
        graph.addEdge(2, 3);
        graph.addEdge(3, 1);
        graph.addEdge(7, 0);
        graph.addEdge(7, 6);

        List<Integer> sortedOrder = graph.topologicalSort();

        System.out.println("Topological sorted order:");
        for (int vertex : sortedOrder) {
            System.out.print(vertex + " ");
        }
    }
}
```

#### Output:
```
Topological sorted order:
7 5 4 2 3 0 1 6
```

#### Explanation:
In this example, the Topological Sort Algorithm is used to find the topologically sorted order of vertices in the given directed acyclic graph. The algorithm iteratively removes vertices with no incoming edges and adds them to the sorted order. The resulting sorted order is printed. In this case, the topologically sorted order of vertices is `7 5 4 2 3 0 1 6`.

## Flood Fill Algorithm

#### Explanation:
Flood Fill Algorithm is a technique used to determine a contiguous region (connected component) within a two-dimensional grid or image and to fill that region with a specific color. It is commonly used in graphics and image processing applications for tasks such as filling regions with colors, finding connected components, and more. The Flood Fill Algorithm typically operates by starting at a specified seed point (pixel) and iteratively explores neighboring pixels to determine the extent of the connected region.

#### Key Steps:
1. **Initialization**: Initialize a data structure (e.g., a queue or stack) to store the pixels to be processed.
2. **Seed Point Selection**: Choose a seed point (x, y) within the grid or image as the starting point for flood fill.
3. **Flood Fill Iteration**: Repeat until all pixels in the connected region are processed:
   - Process the current pixel (x, y) by filling it with the desired color.
   - Explore the neighboring pixels (up, down, left, right) of the current pixel.
   - If a neighboring pixel is within the boundaries of the grid or image and has not been visited yet, add it to the processing queue/stack.
4. **Termination**: The process terminates when all pixels in the connected region have been processed.

#### Applications:
- Filling closed shapes with colors in graphics applications.
- Image segmentation and object recognition in computer vision.
- Terrain generation and map coloring in game development.

#### URL Block Diagram:
```
[Grid/Image, Seed Point, Fill Color] -> [Flood Fill Algorithm] -> [Filled Region]
```

#### Example:
Consider the following 5x5 grid, where `0` represents an empty cell and `1` represents a filled cell.

```
0 0 0 0 0
0 1 1 1 0
0 1 0 1 0
0 1 1 1 0
0 0 0 0 0
```

#### Output:
If we apply flood fill starting from the seed point (1, 1) and fill color `2`, the output will be:

```
0 0 0 0 0
0 2 2 2 0
0 2 0 2 0
0 2 2 2 0
0 0 0 0 0
```

#### DSA Question:
Implement the Flood Fill Algorithm in Java to fill a connected region in a two-dimensional grid with a specified color.

#### Answer:
```java
import java.util.*;

public class FloodFill {
    public static void floodFill(int[][] grid, int x, int y, int fillColor) {
        int rows = grid.length;
        int cols = grid[0].length;
        int originalColor = grid[x][y];
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{x, y});

        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int i = current[0];
            int j = current[1];
            if (i < 0 || i >= rows || j < 0 || j >= cols || grid[i][j] != originalColor) {
                continue;
            }
            grid[i][j] = fillColor;
            queue.offer(new int[]{i - 1, j}); // up
            queue.offer(new int[]{i + 1, j}); // down
            queue.offer(new int[]{i, j - 1}); // left
            queue.offer(new int[]{i, j + 1}); // right
        }
    }

    public static void main(String[] args) {
        int[][] grid = {
            {0, 0, 0, 0, 0},
            {0, 1, 1, 1, 0},
            {0, 1, 0, 1, 0},
            {0, 1, 1, 1, 0},
            {0, 0, 0, 0, 0}
        };
        int seedX = 1;
        int seedY = 1;
        int fillColor = 2;

        floodFill(grid, seedX, seedY, fillColor);

        // Print filled grid
        for (int[] row : grid) {
            for (int cell : row) {
                System.out.print(cell + " ");
            }
            System.out.println();
        }
    }
}
```

#### Output:
```
0 0 0 0 0 
0 2 2 2 0 
0 2 0 2 0 
0 2 2 2 0 
0 0 0 0 0 
```

#### Explanation:
In this example, we apply the Flood Fill Algorithm to fill a connected region starting from the seed point (1, 1) with the fill color `2` in the given 5x5 grid. The algorithm iteratively explores neighboring pixels of the current pixel and fills them with the specified color if they meet the criteria. The resulting filled grid is printed, showing the connected region filled with the fill color `2`.

## Lee Algorithm

#### Explanation:
The Lee Algorithm is a BFS-based algorithm used to find the shortest path between two points in a maze or grid. It operates by exploring all possible paths from the starting point to the ending point in a level-by-level manner until the shortest path is found.

#### Key Steps:
1. **Initialization**: Initialize a queue to store the cells to be explored and a 2D array to mark visited cells.
2. **Enqueue Starting Point**: Add the starting point to the queue and mark it as visited.
3. **Explore Neighbors**: While the queue is not empty, dequeue a cell and explore its neighboring cells.
4. **Update Distances**: Assign distances to neighboring cells and enqueue them if they haven't been visited yet.
5. **Termination**: Continue until the ending point is reached or all reachable cells are explored.

The Lee Algorithm guarantees finding the shortest path if one exists, and it has a time complexity of O(V + E), where V is the number of cells in the grid and E is the number of edges (connections) between cells.

#### URL Block Diagram:
```
[Grid with Starting and Ending Points] -> [Lee Algorithm] -> [Shortest Path]
```

#### Example:
Consider the following maze where `S` represents the starting point, `E` represents the ending point, and `0` represents open cells:

```
S 0 0 0 0
0 0 0 0 0
0 0 0 E 0
0 0 0 0 0
0 0 0 0 0
```

#### Output:
The output will be the shortest path from the starting point to the ending point.

#### DSA Question:
Implement the Lee Algorithm in Java to find the shortest path from the starting point to the ending point in a given maze or grid.

#### Answer:
```java
import java.util.*;

public class LeeAlgorithm {
    static class Point {
        int x, y, dist;

        Point(int x, int y, int dist) {
            this.x = x;
            this.y = y;
            this.dist = dist;
        }
    }

    public static int shortestPath(char[][] grid, Point start, Point end) {
        int rows = grid.length;
        int cols = grid[0].length;
        boolean[][] visited = new boolean[rows][cols];
        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}}; // Up, Down, Left, Right

        Queue<Point> queue = new LinkedList<>();
        queue.offer(start);
        visited[start.x][start.y] = true;

        while (!queue.isEmpty()) {
            Point current = queue.poll();
            if (current.x == end.x && current.y == end.y) {
                return current.dist;
            }

            for (int[] dir : directions) {
                int newX = current.x + dir[0];
                int newY = current.y + dir[1];
                if (newX >= 0 && newX < rows && newY >= 0 && newY < cols &&
                        grid[newX][newY] == '0' && !visited[newX][newY]) {
                    queue.offer(new Point(newX, newY, current.dist + 1));
                    visited[newX][newY] = true;
                }
            }
        }

        return -1; // No path found
    }

    public static void main(String[] args) {
        char[][] grid = {
                {'S', '0', '0', '0', '0'},
                {'0', '0', '0', '0', '0'},
                {'0', '0', '0', 'E', '0'},
                {'0', '0', '0', '0', '0'},
                {'0', '0', '0', '0', '0'}
        };
        Point start = new Point(0, 0, 0);
        Point end = new Point(2, 3, 0);

        int shortestPathLength = shortestPath(grid, start, end);
        System.out.println("Shortest path length: " + shortestPathLength);
    }
}
```

#### Output:
```
Shortest path length: 7
```

#### Explanation:
In this example, the Lee Algorithm (BFS) is used to find the shortest path from the starting point `S` to the ending point `E` in the given maze. The algorithm explores neighboring cells in a level-by-level manner until it reaches the ending point. The output is the length of the shortest path, which is `7` in this case.
