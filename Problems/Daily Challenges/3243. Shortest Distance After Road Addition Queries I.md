[Shortest Distance After Road Addition Queries I](https://leetcode.com/problems/shortest-distance-after-road-addition-queries-i/)

# Intuition:

The problem requires finding the shortest path dynamically as new edges are added. A Breadth-First Search (BFS)-based approach can efficiently determine the shortest path in an unweighted graph after each query. Instead of maintaining and recalculating the entire graph structure for every query, BFS only processes reachable nodes, making it more time-efficient compared to algorithms like Floyd-Warshall for this problem.

# Approach:

1. **Graph Initialization**:
    
    - Represent the graph as an adjacency list (`g`), with initial edges connecting each city *i* to *i+1*.

2. **Breadth-First Search (BFS)**:
    - Start from city *0* and traverse the graph using BFS to determine the shortest path to city **n - 1**.
    - Use a queue to manage nodes at the current level and a visited array to avoid revisiting nodes.

3. **Processing Queries**:    
    - For each query **[u, v]**, add the new road *u → v* to the graph.
    - Perform BFS from city *0* and record the shortest distance to city *n − 1*.

4. **Collect Results**:
    - Store the shortest path distance after each query in the `ans` array.

# Complexity:

1. **Time Complexity**:
    - Adding an edge: ***O(1)***.
    - BFS per query: ***O(n + m)***, where mm is the number of edges.
    - Total for qq queries: ***O(q ⋅ (n + m))***.
    - With constraints ***n, q ≤ 500***, this approach is efficient.
	
2. **Space Complexity**:
    - Adjacency list: ***O(n + m)***.
    - BFS queue and visited array: ***O(n)***.

# Code

```typescript
function shortestDistanceAfterQueries(n: number, queries: number[][]): number[] {
    // Initialize graph as an adjacency list
    const g: number[][] = Array.from({ length: n }, () => []);
    for (let i = 0; i < n - 1; ++i) {
        g[i].push(i + 1);
    }

    // BFS function to find shortest path from 0 to n-1
    const bfs = (start: number): number => {
        const queue: number[] = [start];
        const visited: boolean[] = Array(n).fill(false);
        visited[start] = true;

        for (let distance = 0; ; ++distance) {
            const nextQueue: number[] = [];
            for (const current of queue) {
                if (current === n - 1) return distance;
                for (const neighbor of g[current]) {
                    if (!visited[neighbor]) {
                        visited[neighbor] = true;
                        nextQueue.push(neighbor);
                    }
                }
            }
            queue.splice(0, queue.length, ...nextQueue);
        }
    };

    // Process each query and calculate the shortest path
    const result: number[] = [];
    for (const [u, v] of queries) {
        g[u].push(v); // Add new road
        result.push(bfs(0)); // Calculate shortest path from 0 to n-1
    }

    return result;
};

```

---

### Example Walkthrough:

#### Example 1:

**Input**:

```typescript
n = 5;
queries = [[2, 4], [0, 2], [0, 4]];
```

**Execution**:

1. **Initial graph**:
    
    ```
    0 -> 1 -> 2 -> 3 -> 4
    ```
    
    - Shortest path from **0** to **4**: **3**.
2. **Query 1**: Add road **2→4**.
    
    ```
    0 -> 1 -> 2 -> 3 -> 4
             \---------> 4
    ```
    
    - BFS from **0**: Shortest path becomes **3**.
3. **Query 2**: Add road **0→2**.
    
    ```
    0 -> 1 -> 2 -> 3 -> 4
        \------> 2
             \---------> 4
    ```
    
    - BFS from **0**: Shortest path becomes **2**.
4. **Query 3**: Add road **0→4**.
    
    ```
    0 -> 1 -> 2 -> 3 -> 4
        \------> 2
        \----------------> 4
    ```
    
    - BFS from **0**: Shortest path becomes **1**.

**Output**:

```typescript
[3, 2, 1]
```

#### Example 2:

**Input**:

```typescript
n = 4;
queries = [[0, 3], [0, 2]];
```

**Execution**:

1. **Initial graph**:
    
    ```
    0 -> 1 -> 2 -> 3
    ```
    
    - Shortest path from **0** to **3**: **3**.
2. **Query 1**: Add road **0→3**.
    
    ```
    0 -> 1 -> 2 -> 3
        \------------> 3
    ```
    
    - BFS from **0**: Shortest path becomes **1**.
3. **Query 2**: Add road **0→2**.
    
    ```
    0 -> 1 -> 2 -> 3
        \-----> 2
        \------------> 3
    ```
    
    - BFS from **0**: Shortest path remains **1**.

**Output**:

```typescript
[1, 1]
```

---

### Key Insights:

- Using BFS for each query is optimal because it dynamically computes the shortest path without recalculating unnecessary paths.
- This approach leverages the graph's sparse nature and localized changes due to each query.