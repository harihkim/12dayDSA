# Day 09 — Graph Traversal

## The Pattern

**What it does**: Explores connected nodes/cells using BFS (shortest path, level-by-level) or DFS (explore all paths, mark regions).
**When to reach for it**: You have a grid, adjacency list, or any structure where nodes connect to neighbors.

---

## Recognition Triggers

- "Number of islands / connected regions" → Grid DFS/BFS
- "Shortest path (unweighted)" → BFS
- "Flood fill / paint" → DFS/BFS from source
- "Rotting oranges / spreading fire" → Multi-source BFS
- "Word ladder / transformation" → BFS (shortest chain)
- "Clone graph" → BFS/DFS + hash map
- "Pacific Atlantic water flow" → Reverse DFS from edges

---

## The Template

### Build Adjacency List
```python
from collections import defaultdict

def build_graph(edges, directed=False):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        if not directed:
            graph[v].append(u)
    return graph
```

### BFS (Shortest Path)
```python
from collections import deque

def bfs(graph, start):
    visited = {start}
    queue = deque([(start, 0)])  # (node, distance)

    while queue:
        node, dist = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, dist + 1))

    return visited
```

### DFS (Explore All)
```python
def dfs(graph, node, visited):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

### Grid DFS — Number of Islands
```python
def num_islands(grid):
    rows, cols = len(grid), len(grid[0])
    count = 0

    def dfs(r, c):
        if r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] == '0':
            return
        grid[r][c] = '0'  # mark visited
        for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
            dfs(r + dr, c + dc)

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                dfs(r, c)
                count += 1
    return count
```

### Multi-Source BFS — Rotting Oranges
```python
from collections import deque

def oranges_rotting(grid):
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh = 0

    # Seed BFS with ALL rotten oranges
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c, 0))
            elif grid[r][c] == 1:
                fresh += 1

    time = 0
    while queue:
        r, c, t = queue.popleft()
        for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                grid[nr][nc] = 2
                fresh -= 1
                time = t + 1
                queue.append((nr, nc, t + 1))

    return time if fresh == 0 else -1
```

### Clone Graph
```python
from collections import deque

def clone_graph(node):
    if not node:
        return None
    cloned = {node: Node(node.val)}
    queue = deque([node])

    while queue:
        curr = queue.popleft()
        for neighbor in curr.neighbors:
            if neighbor not in cloned:
                cloned[neighbor] = Node(neighbor.val)
                queue.append(neighbor)
            cloned[curr].neighbors.append(cloned[neighbor])

    return cloned[node]
```

---

## Grid Traversal Utilities
```python
# 4-directional movement
DIRS = [(0,1),(0,-1),(1,0),(-1,0)]

def in_bounds(r, c, rows, cols):
    return 0 <= r < rows and 0 <= c < cols
```

---

## BFS vs DFS on Graphs

| | BFS | DFS |
|-|-----|-----|
| **Finds** | Shortest path (unweighted) | All reachable nodes |
| **Structure** | Queue | Recursion / stack |
| **Memory** | O(width of frontier) | O(depth of graph) |
| **Use when** | Need shortest distance | Need to explore all / mark regions |

---

## Common Traps
- Forgetting to **mark visited BEFORE queueing** in BFS → processes same node multiple times
- Modifying grid in-place is convenient but **mutates input** → make a copy if needed
- Multi-source BFS: forgetting to seed **all sources** at once → gives wrong distances
- Using DFS for shortest path in unweighted graph → BFS guarantees shortest, DFS does not
