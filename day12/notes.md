# Day 12 — Backtracking & Greedy

Two patterns that sit at opposite ends of the spectrum:
- **Backtracking**: Try everything systematically. Guaranteed correct but slow.
- **Greedy**: Take the best option now. Fast but only works when local = global.

---

## Part 1: Backtracking

### Recognition Triggers
- "Generate all subsets / combinations / permutations"
- "Find all valid configurations" (N-Queens, Sudoku)
- "Partition into groups" (palindrome partitioning)
- "Word search in a grid"

### The Template
```
def backtrack(state, choices):
    if GOAL_REACHED(state):
        result.append(state.copy())
        return

    for choice in choices:
        if is_valid(choice):
            MAKE(choice)              # choose
            backtrack(next_state)     # explore
            UNDO(choice)              # unchoose
```

### Subsets
```python
def subsets(nums):
    result = []
    def backtrack(start, current):
        result.append(current[:])     # every state is valid
        for i in range(start, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()             # unchoose
    backtrack(0, [])
    return result
```

### Permutations
```python
def permutations(nums):
    result = []
    def backtrack(current, remaining):
        if not remaining:
            result.append(current[:])
            return
        for i in range(len(remaining)):
            current.append(remaining[i])
            backtrack(current, remaining[:i] + remaining[i+1:])
            current.pop()
    backtrack([], nums)
    return result
```

### Combination Sum (reuse allowed)
```python
def combination_sum(candidates, target):
    result = []
    def backtrack(start, current, remaining):
        if remaining == 0:
            result.append(current[:])
            return
        for i in range(start, len(candidates)):
            if candidates[i] > remaining:
                break
            current.append(candidates[i])
            backtrack(i, current, remaining - candidates[i])  # i, not i+1: reuse
            current.pop()
    candidates.sort()
    backtrack(0, [], target)
    return result
```

### Subsets vs Permutations vs Combinations

| Type | Order matters? | Reuse? | Loop starts at |
|------|---------------|--------|---------------|
| **Subsets** | No | No | `start` (skip previous) |
| **Combinations** | No | Optional | `start` or `i` |
| **Permutations** | Yes | No | `0` (pick from remaining) |

---

## Part 2: Greedy

### Recognition Triggers
- "Merge overlapping intervals" → sort + extend
- "Minimum number of intervals to remove" → sort by end
- "Jump game / reach the end" → track max reach
- "Assign cookies / tasks optimally" → sort + match
- "Activity selection / meeting rooms" → sort by end time

### When Does Greedy Work?
Only when the **greedy choice property** holds: making the locally optimal choice at each step leads to a globally optimal solution. If you're not sure, try to find a counterexample.

### Merge Intervals
```python
def merge(intervals):
    intervals.sort(key=lambda x: x[0])
    merged = [intervals[0]]
    for start, end in intervals[1:]:
        if start <= merged[-1][1]:
            merged[-1][1] = max(merged[-1][1], end)
        else:
            merged.append([start, end])
    return merged
```

### Non-Overlapping Intervals (Minimize Removals)
```python
def erase_overlap(intervals):
    intervals.sort(key=lambda x: x[1])  # sort by END time
    count = 0
    prev_end = float('-inf')
    for start, end in intervals:
        if start >= prev_end:
            prev_end = end    # keep this interval
        else:
            count += 1        # remove (overlaps)
    return count
```

**Why sort by end?** Choosing the interval that ends earliest leaves the most room for future intervals.

### Jump Game
```python
def can_jump(nums):
    max_reach = 0
    for i in range(len(nums)):
        if i > max_reach:
            return False
        max_reach = max(max_reach, i + nums[i])
    return True
```

---

## Backtracking vs Greedy

| | Backtracking | Greedy |
|-|-------------|--------|
| **Explores** | All possibilities | One best choice |
| **Guarantee** | Always correct | Correct only with greedy property |
| **Time** | Exponential | Usually O(n log n) |
| **Use when** | Need ALL solutions or no greedy property | Greedy property provable |

---

## Common Traps

### Backtracking
- Forgetting to **undo the choice** (pop) → corrupted state
- Not **sorting** before combination problems → can't prune or skip duplicates
- Skipping duplicates incorrectly → `if i > start and nums[i] == nums[i-1]: continue`
- Using `result.append(current)` instead of `result.append(current[:])` → all entries share same list

### Greedy
- Assuming greedy works without proof → test with counterexample first
- Sorting by the **wrong key** — end time for scheduling, start time for merging
- Not handling **ties** in the sort
