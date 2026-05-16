# 🧠 12-Day DSA Pattern Mastery

> **Philosophy**: One pattern per day. Go deep, not wide. Every DSA interview problem is a variation of these 12 patterns. Master recognition → apply template → adapt to constraints.
>
> **Pace**: ~3–4 hours/day | **Language**: Python

---

## The 12 Patterns

| Day | Pattern | Recognition Trigger |
|-----|---------|-------------------|
| 01 | **Hash Map** | "find pair", "count frequency", "group by", "duplicates" |
| 02 | **Prefix Sum** | "subarray sum", "range query", "cumulative" |
| 03 | **Two Pointers** | "sorted array", "pair sum", "palindrome", "triplet" |
| 04 | **Sliding Window** | "contiguous subarray/substring", "longest/shortest with condition" |
| 05 | **Stack** | "next greater/smaller", "valid brackets", "nested structure" |
| 06 | **Linked List** | "reverse", "merge", "cycle", "middle node" |
| 07 | **Tree DFS/BFS** | "depth", "path sum", "level order", "validate BST" |
| 08 | **Binary Search** | "sorted data", "minimize maximum", "find boundary" |
| 09 | **Graph Traversal** | "connected regions", "shortest path", "flood fill" |
| 10 | **Topo Sort & Union-Find** | "dependencies", "ordering", "connected components" |
| 11 | **Dynamic Programming** | "how many ways", "min/max cost", "can it be done" |
| 12 | **Backtracking & Greedy** | "all subsets/permutations" or "intervals/scheduling" |

---

## 🧭 Pattern Recognition Flowchart

```
New problem → Read it → Ask yourself:

Is the input sorted / can sorting help?
├─ YES, find target/pair → Two Pointers (Day 03)
├─ YES, minimize/maximize → Binary Search (Day 08)
└─ NO
   ├─ Need O(1) lookup or counting? → Hash Map (Day 01)
   ├─ Subarray/range sum? → Prefix Sum (Day 02)
   ├─ Contiguous subarray with constraint? → Sliding Window (Day 04)
   ├─ Next greater/smaller or nesting? → Stack (Day 05)
   ├─ Linked list manipulation? → Linked List (Day 06)
   ├─ Tree structure? → DFS / BFS (Day 07)
   ├─ Grid or graph connections? → Graph Traversal (Day 09)
   ├─ Dependencies or grouping? → Topo Sort / Union-Find (Day 10)
   ├─ Overlapping subproblems + optimal? → DP (Day 11)
   └─ All combos/perms or local = global? → Backtracking / Greedy (Day 12)
```

---

## Daily Structure

Each day follows the same rhythm:

| Phase | Time | What You Do |
|-------|------|-------------|
| **Learn** | 45 min | Read `notes.md` — understand the pattern, why it works, template |
| **Drill** | 90 min | Solve 4–5 problems, easiest first. Before coding: name the pattern |
| **Reflect** | 30 min | For each problem, write a one-line note: "I recognized this because..." |
| **Stretch** | 45 min | Attempt 1 hard problem or a variant you haven't seen |

---

## Day-by-Day Map

### Day 01 — Hash Map
> *"I need fast lookup, counting, or finding a complement."*

📖 [day01/notes.md](day01/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Two Sum | Complement lookup in O(1) | [#1](https://leetcode.com/problems/two-sum/) |
| Valid Anagram | Frequency comparison | [#242](https://leetcode.com/problems/valid-anagram/) |
| Group Anagrams | Canonical key → group | [#49](https://leetcode.com/problems/group-anagrams/) |
| Top K Frequent Elements | Frequency count → bucket sort | [#347](https://leetcode.com/problems/top-k-frequent-elements/) |
| Longest Consecutive Sequence | Set membership for O(1) neighbor check | [#128](https://leetcode.com/problems/longest-consecutive-sequence/) |

---

### Day 02 — Prefix Sum
> *"I need subarray sums or range queries without re-computing."*

📖 [day02/notes.md](day02/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Running Sum of 1D Array | Pure prefix sum | [#1480](https://leetcode.com/problems/running-sum-of-1d-array/) |
| Product of Array Except Self | Prefix × suffix products | [#238](https://leetcode.com/problems/product-of-array-except-self/) |
| Subarray Sum Equals K | Prefix sum + hash map combo | [#560](https://leetcode.com/problems/subarray-sum-equals-k/) |
| Contiguous Array | Prefix sum with +1/−1 transform | [#525](https://leetcode.com/problems/contiguous-array/) |
| Range Sum Query (Immutable) | Classic prefix sum application | [#303](https://leetcode.com/problems/range-sum-query-immutable/) |

---

### Day 03 — Two Pointers
> *"The array is sorted (or I can sort it), and I need pairs or sections."*

📖 [day03/notes.md](day03/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Valid Palindrome | Converge from both ends | [#125](https://leetcode.com/problems/valid-palindrome/) |
| Two Sum II (Sorted) | Sorted → converging pointers | [#167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) |
| 3Sum | Fix one + two pointer scan | [#15](https://leetcode.com/problems/3sum/) |
| Container With Most Water | Move the shorter wall inward | [#11](https://leetcode.com/problems/container-with-most-water/) |
| Trapping Rain Water | Two pointers tracking max heights | [#42](https://leetcode.com/problems/trapping-rain-water/) |

---

### Day 04 — Sliding Window
> *"I need the longest/shortest/best contiguous subarray or substring under some constraint."*

📖 [day04/notes.md](day04/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Best Time to Buy and Sell Stock | Track running minimum (implicit window) | [#121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) |
| Longest Substring Without Repeating | Variable window — expand/contract on duplicates | [#3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |
| Permutation in String | Fixed window — anagram check | [#567](https://leetcode.com/problems/permutation-in-string/) |
| Longest Repeating Character Replacement | Variable window — track max frequency | [#424](https://leetcode.com/problems/longest-repeating-character-replacement/) |
| Minimum Window Substring | Variable window — contract when valid | [#76](https://leetcode.com/problems/minimum-window-substring/) |

---

### Day 05 — Stack
> *"I need to track a nesting structure, or find the next greater/smaller element."*

📖 [day05/notes.md](day05/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Valid Parentheses | Stack matching — push open, pop close | [#20](https://leetcode.com/problems/valid-parentheses/) |
| Min Stack | Auxiliary stack tracks min at each level | [#155](https://leetcode.com/problems/min-stack/) |
| Daily Temperatures | Monotonic decreasing stack | [#739](https://leetcode.com/problems/daily-temperatures/) |
| Next Greater Element I | Monotonic stack + hash map | [#496](https://leetcode.com/problems/next-greater-element-i/) |
| Largest Rectangle in Histogram | Monotonic increasing stack | [#84](https://leetcode.com/problems/largest-rectangle-in-histogram/) |

---

### Day 06 — Linked List
> *"I need to reverse, merge, detect a cycle, or find the middle."*

📖 [day06/notes.md](day06/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Reverse Linked List | Three-pointer swap: prev/curr/next | [#206](https://leetcode.com/problems/reverse-linked-list/) |
| Merge Two Sorted Lists | Dummy node + comparison walk | [#21](https://leetcode.com/problems/merge-two-sorted-lists/) |
| Linked List Cycle | Floyd's fast/slow | [#141](https://leetcode.com/problems/linked-list-cycle/) |
| Middle of the Linked List | Fast/slow — slow lands at middle | [#876](https://leetcode.com/problems/middle-of-the-linked-list/) |
| Reorder List | Find middle + reverse second half + merge | [#143](https://leetcode.com/problems/reorder-list/) |

---

### Day 07 — Tree DFS / BFS
> *"I need to traverse, validate, or compute something over a tree."*

📖 [day07/notes.md](day07/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Invert Binary Tree | DFS — swap children at each node | [#226](https://leetcode.com/problems/invert-binary-tree/) |
| Maximum Depth | DFS — `1 + max(left, right)` | [#104](https://leetcode.com/problems/maximum-depth-of-binary-tree/) |
| Level Order Traversal | BFS — queue with level grouping | [#102](https://leetcode.com/problems/binary-tree-level-order-traversal/) |
| Validate BST | DFS — pass valid range (lo, hi) | [#98](https://leetcode.com/problems/validate-binary-search-tree/) |
| Lowest Common Ancestor | DFS — split point is the LCA | [#236](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) |

---

### Day 08 — Binary Search
> *"The data is sorted or the answer space is monotonic — I can halve it."*

📖 [day08/notes.md](day08/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Binary Search | Classic template | [#704](https://leetcode.com/problems/binary-search/) |
| Search in Rotated Sorted Array | BS with pivot detection | [#33](https://leetcode.com/problems/search-in-rotated-sorted-array/) |
| Find Minimum in Rotated Sorted Array | BS — compare mid with right | [#153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) |
| Koko Eating Bananas | BS on answer — min speed | [#875](https://leetcode.com/problems/koko-eating-bananas/) |
| Median of Two Sorted Arrays | BS on partition | [#4](https://leetcode.com/problems/median-of-two-sorted-arrays/) |

---

### Day 09 — Graph Traversal
> *"I have a grid or graph and need to explore connected regions or shortest path."*

📖 [day09/notes.md](day09/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Number of Islands | Grid DFS — sink visited land | [#200](https://leetcode.com/problems/number-of-islands/) |
| Clone Graph | BFS + old→new hash map | [#133](https://leetcode.com/problems/clone-graph/) |
| Rotting Oranges | Multi-source BFS — simultaneous spread | [#994](https://leetcode.com/problems/rotting-oranges/) |
| Pacific Atlantic Water Flow | Reverse DFS from both oceans | [#417](https://leetcode.com/problems/pacific-atlantic-water-flow/) |
| Word Ladder | BFS — shortest transformation path | [#127](https://leetcode.com/problems/word-ladder/) |

---

### Day 10 — Topological Sort & Union-Find
> *"I have dependencies to order, or components to group."*

📖 [day10/notes.md](day10/notes.md)

| Problem | Why This Pattern | LeetCode |
|---------|-----------------|----------|
| Course Schedule | Topo sort — cycle = impossible | [#207](https://leetcode.com/problems/course-schedule/) |
| Course Schedule II | Topo sort — return valid ordering | [#210](https://leetcode.com/problems/course-schedule-ii/) |
| Alien Dictionary | Topo sort from character ordering | [#269](https://leetcode.com/problems/alien-dictionary/) |
| Number of Connected Components | Union-Find — count roots | [#323](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) |
| Redundant Connection | Union-Find — edge that creates a cycle | [#684](https://leetcode.com/problems/redundant-connection/) |

---

### Day 11 — Dynamic Programming
> *"Overlapping subproblems + optimal substructure. How many ways? Min cost? Is it possible?"*

📖 [day11/notes.md](day11/notes.md)

| Problem | DP Sub-Pattern | LeetCode |
|---------|---------------|----------|
| Climbing Stairs | Fibonacci DP | [#70](https://leetcode.com/problems/climbing-stairs/) |
| House Robber | Decision DP — take or skip | [#198](https://leetcode.com/problems/house-robber/) |
| Coin Change | Unbounded Knapsack | [#322](https://leetcode.com/problems/coin-change/) |
| Longest Increasing Subsequence | LIS pattern | [#300](https://leetcode.com/problems/longest-increasing-subsequence/) |
| Longest Common Subsequence | 2D String DP | [#1143](https://leetcode.com/problems/longest-common-subsequence/) |

---

### Day 12 — Backtracking & Greedy
> *"Generate all combos/permutations" → Backtracking. "Local optimal = global optimal" → Greedy.*

📖 [day12/notes.md](day12/notes.md)

| Problem | Sub-Pattern | LeetCode |
|---------|------------|----------|
| Subsets | Backtracking — include/exclude | [#78](https://leetcode.com/problems/subsets/) |
| Permutations | Backtracking — pick from remaining | [#46](https://leetcode.com/problems/permutations/) |
| Combination Sum | Backtracking — reuse allowed | [#39](https://leetcode.com/problems/combination-sum/) |
| Merge Intervals | Greedy — sort + extend | [#56](https://leetcode.com/problems/merge-intervals/) |
| Jump Game | Greedy — track max reach | [#55](https://leetcode.com/problems/jump-game/) |

---

## 📌 How to Study Each Day

1. **Read `notes.md`** — understand the pattern, not just the code
2. **Memorize the template** — write it from scratch on paper
3. **Solve problems** — before coding, say *"this is pattern X because..."*
4. **If stuck > 15 min** — re-read the template, then retry
5. **Reflect** — write one line per problem: *"I recognized this because..."*

> **The goal isn't "I solved 60 problems." It's "I see a new problem and instantly know which pattern to reach for."**

---

## 🏗️ Folder Structure

```
7day/
├── README.md
├── day01/  ← Hash Map
├── day02/  ← Prefix Sum
├── day03/  ← Two Pointers
├── day04/  ← Sliding Window
├── day05/  ← Stack
├── day06/  ← Linked List
├── day07/  ← Tree DFS/BFS
├── day08/  ← Binary Search
├── day09/  ← Graph Traversal
├── day10/  ← Topo Sort & Union-Find
├── day11/  ← Dynamic Programming
└── day12/  ← Backtracking & Greedy
```

Each `dayNN/` contains:
- `notes.md` — Pattern explanation, template, recognition triggers, variations
- `problems/` — Your solutions
