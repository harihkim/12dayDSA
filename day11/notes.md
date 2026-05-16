# Day 11 — Dynamic Programming

## The Pattern

**What it does**: Breaks a problem into overlapping subproblems, solves each once, and combines results.
**When to reach for it**: The problem has optimal substructure AND overlapping subproblems — "how many ways", "min/max cost", "is it possible".

---

## Recognition Triggers

- "How many ways to reach…" → DP (count paths)
- "Minimum cost / coins / edits" → DP (minimize)
- "Maximum profit / length" → DP (maximize)
- "Can you partition / reach / break?" → DP (feasibility)
- Choices at each step: take/skip, left/right, use/don't use
- Answer depends on **smaller versions** of the same question

---

## The Framework

```
1. DEFINE subproblem: What does dp[i] (or dp[i][j]) represent?
2. RECURRENCE: How does dp[i] relate to smaller subproblems?
3. BASE CASE: What are the trivial answers?
4. ORDER: In what order do you fill the table?
5. ANSWER: Which cell contains the final answer?
```

---

## Sub-Patterns & Templates

### 1. Fibonacci DP
**Signal**: dp[i] depends on dp[i-1] and dp[i-2].

```python
def climb_stairs(n):
    if n <= 2: return n
    prev2, prev1 = 1, 2
    for i in range(3, n + 1):
        curr = prev1 + prev2
        prev2, prev1 = prev1, curr
    return prev1
```

### 2. Decision DP (Take or Skip)
**Signal**: At each element, choose to include or exclude it.

```python
def rob(nums):
    prev2, prev1 = 0, 0
    for num in nums:
        curr = max(prev1, prev2 + num)  # skip or take
        prev2, prev1 = prev1, curr
    return prev1
```

**Recurrence**: `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`

### 3. Unbounded Knapsack
**Signal**: Unlimited supply of items, minimize/maximize value for a target.

```python
def coin_change(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1
```

**Recurrence**: `dp[i] = min(dp[i - coin] + 1)` for each valid coin

### 4. Longest Increasing Subsequence (LIS)
**Signal**: Find the longest subsequence with a monotonic property.

```python
# O(n²) DP
def length_of_lis(nums):
    dp = [1] * len(nums)
    for i in range(1, len(nums)):
        for j in range(i):
            if nums[j] < nums[i]:
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp)

# O(n log n) with binary search
import bisect
def lis_fast(nums):
    tails = []
    for num in nums:
        pos = bisect.bisect_left(tails, num)
        if pos == len(tails):
            tails.append(num)
        else:
            tails[pos] = num
    return len(tails)
```

### 5. 2D Grid DP
**Signal**: Grid paths, minimum cost path, counting routes.

```python
def unique_paths(m, n):
    dp = [[1] * n for _ in range(m)]
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
    return dp[m-1][n-1]
```

### 6. 2D String DP
**Signal**: Two strings, comparing characters — LCS, edit distance.

```python
def lcs(text1, text2):
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[m][n]
```

---

## Top-Down vs Bottom-Up

| | Top-Down (Memo) | Bottom-Up (Table) |
|-|----------------|-------------------|
| **Style** | Recursive + cache | Iterative + array |
| **Pros** | Only solves needed subproblems | No recursion overhead |
| **Cons** | Stack overflow risk | Must determine fill order |
| **Convert** | Memo → identify dependencies → tabulate |

### Top-Down Template
```python
from functools import lru_cache

@lru_cache(maxsize=None)
def solve(i, ...):
    if BASE_CASE:
        return ...
    return COMBINE(solve(i-1, ...), solve(i-2, ...), ...)
```

---

## DP vs Greedy vs Backtracking

| | DP | Greedy | Backtracking |
|-|----|--------|-------------|
| **Explores** | All choices (memoized) | Best local choice only | All choices (no memo) |
| **Optimal?** | Always (if applicable) | Only with greedy property | Always (brute force) |
| **Speed** | Polynomial | Fast (often O(n log n)) | Exponential |

---

## Common Traps
- Not recognizing DP — "how many ways" is almost always DP
- Defining `dp[i]` wrong — the definition must make the recurrence possible
- Forgetting **base cases** — `dp[0]`, `dp[1]`, or `dp[0][0]`
- 2D problems: **index shifting** — `text1[i-1]` not `text1[i]` when dp is (m+1)×(n+1)
- Space optimization: only works when dp[i] depends on **previous row/few values only**
