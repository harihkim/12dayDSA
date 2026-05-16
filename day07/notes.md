# Day 07 — Tree DFS / BFS

## The Pattern

**What it does**: Traverses a tree structure either depth-first (go deep, then backtrack) or breadth-first (level by level).
**When to reach for it**: The input is a tree and you need to compute depth, validate structure, find paths, or process levels.

---

## Recognition Triggers

- "Maximum depth / height" → DFS, return `1 + max(left, right)`
- "Path sum / root to leaf" → DFS, carry accumulated value
- "Validate BST" → DFS, pass valid range `(lo, hi)` downward
- "Invert / mirror" → DFS, swap children at each node
- "Level order traversal" → BFS with queue
- "Right side view / zigzag" → BFS, track level boundaries
- "Lowest common ancestor" → DFS, check left/right returns

---

## The Template

### DFS — Recursive
```python
def dfs(node):
    if not node:
        return BASE_CASE

    left = dfs(node.left)
    right = dfs(node.right)

    return COMBINE(node.val, left, right)
```

### Maximum Depth
```python
def max_depth(root):
    if not root:
        return 0
    return 1 + max(max_depth(root.left), max_depth(root.right))
```

### Invert Binary Tree
```python
def invert(root):
    if not root:
        return None
    root.left, root.right = invert(root.right), invert(root.left)
    return root
```

### Validate BST
```python
def is_valid_bst(node, lo=float('-inf'), hi=float('inf')):
    if not node:
        return True
    if node.val <= lo or node.val >= hi:
        return False
    return (is_valid_bst(node.left, lo, node.val) and
            is_valid_bst(node.right, node.val, hi))
```

### BFS — Level Order
```python
from collections import deque

def level_order(root):
    if not root:
        return []
    result = []
    queue = deque([root])
    while queue:
        level = []
        for _ in range(len(queue)):    # process current level
            node = queue.popleft()
            level.append(node.val)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level)
    return result
```

### Lowest Common Ancestor
```python
def lca(root, p, q):
    if not root or root == p or root == q:
        return root
    left = lca(root.left, p, q)
    right = lca(root.right, p, q)
    if left and right:
        return root      # split point → this is the LCA
    return left or right  # one side found it
```

---

## DFS Traversal Orders

```
        1
       / \
      2   3
     / \
    4   5
```

| Order | Visit Sequence | Use Case |
|-------|---------------|----------|
| **Preorder** (Root→L→R) | 1, 2, 4, 5, 3 | Copy tree, serialize |
| **Inorder** (L→Root→R) | 4, 2, 5, 1, 3 | BST → sorted order |
| **Postorder** (L→R→Root) | 4, 5, 2, 3, 1 | Delete tree, calc size |

---

## DFS vs BFS

| | DFS | BFS |
|-|-----|-----|
| **Structure** | Recursion / explicit stack | Queue |
| **Memory** | O(height) | O(max width) |
| **Best for** | Path problems, validation | Level problems, shortest depth |
| **Balanced tree** | O(log n) memory | O(n) memory |
| **Skewed tree** | O(n) memory | O(1) memory |

---

## Common Traps
- Forgetting the **base case** `if not node: return` → infinite recursion
- Validate BST using `node.left.val < node.val` only checks immediate child, not entire subtree → pass range instead
- BFS: forgetting the `for _ in range(len(queue))` loop → can't distinguish levels
- Confusing **preorder vs inorder vs postorder** — matters for serialization and BST problems
