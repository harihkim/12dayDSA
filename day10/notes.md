# Day 10 — Topological Sort & Union-Find

## The Pattern

Two distinct tools for two distinct graph problems:
- **Topological Sort**: Order nodes in a DAG so dependencies come first.
- **Union-Find**: Group nodes into connected components efficiently.

---

## Recognition Triggers

### Topological Sort
- "Course schedule / prerequisites" → can you finish? what order?
- "Build order / task dependencies" → dependency ordering
- "Alien dictionary / character ordering" → derive order from comparisons
- "Detect cycle in directed graph" → topo sort fails if cycle exists

### Union-Find
- "Number of connected components" → count distinct roots
- "Redundant connection" → edge that creates a cycle
- "Accounts merge / synonyms" → group equivalent items
- "Detect cycle in undirected graph" → union fails if already connected

---

## The Template

### Topological Sort — Kahn's BFS
```python
from collections import deque, defaultdict

def topo_sort(num_nodes, edges):
    graph = defaultdict(list)
    indegree = [0] * num_nodes

    for u, v in edges:          # u must come before v
        graph[u].append(v)
        indegree[v] += 1

    queue = deque([i for i in range(num_nodes) if indegree[i] == 0])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)

    if len(order) != num_nodes:
        return []       # cycle detected — no valid ordering
    return order
```

**Why it works**: Start from nodes with no prerequisites (indegree 0). Process them, reduce neighbors' indegree. When a neighbor hits 0, it's ready. If some nodes never reach 0, there's a cycle.

### Course Schedule (Can Finish?)
```python
def can_finish(num_courses, prerequisites):
    order = topo_sort(num_courses, prerequisites)
    return len(order) == num_courses
```

### Union-Find
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.components = n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False        # already in same component
        # Union by rank
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        self.components -= 1
        return True

    def connected(self, x, y):
        return self.find(x) == self.find(y)
```

### Connected Components
```python
def count_components(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        uf.union(u, v)
    return uf.components
```

### Redundant Connection
```python
def find_redundant(edges):
    uf = UnionFind(len(edges) + 1)
    for u, v in edges:
        if not uf.union(u, v):
            return [u, v]  # this edge creates a cycle
    return []
```

---

## Topo Sort vs Union-Find

| | Topo Sort | Union-Find |
|-|-----------|------------|
| **Graph type** | Directed (DAG) | Undirected |
| **Purpose** | Ordering | Grouping |
| **Cycle detection** | Yes (incomplete ordering) | Yes (union returns False) |
| **Time** | O(V + E) | O(α(n)) per operation ≈ O(1) |

---

## Union-Find Optimizations

| Optimization | What it does | Effect |
|-------------|-------------|--------|
| **Path compression** | Point nodes directly to root during `find` | Nearly O(1) per find |
| **Union by rank** | Attach shorter tree under taller tree | Keeps tree balanced |
| **Both together** | Amortized O(α(n)) per operation | α(n) ≤ 4 for all practical n |

---

## Common Traps
- Topo sort: mixing up **edge direction** — `(u, v)` means u must come before v
- Topo sort: forgetting to check `len(order) == num_nodes` for **cycle detection**
- Union-Find: forgetting **path compression** → degrades to O(n)
- Union-Find: using on **directed** graphs — it's for undirected only
- Union-Find: off-by-one if nodes are 1-indexed but array is 0-indexed
