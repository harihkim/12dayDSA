# Day 01 — Hash Map

## The Pattern

**What it does**: Trades O(n) space for O(1) lookup time.
**When to reach for it**: You need to find something fast — a complement, a duplicate, a frequency, or a group.

---

## Recognition Triggers

- "Find two numbers that add up to X" → complement lookup
- "Are these anagrams?" → frequency comparison
- "Group elements by some property" → canonical key → hash map
- "Find duplicates" → seen set
- "Count occurrences" → Counter

---

## The Template

### Complement Lookup (Two Sum Pattern)
```python
def find_complement(nums, target):
    seen = {}  # value → index
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []
```

**Why it works**: Instead of checking every pair O(n²), we store what we've seen and ask "have I already seen my complement?"

### Frequency Counter
```python
from collections import Counter

def top_k_frequent(nums, k):
    count = Counter(nums)
    # Bucket sort: index = frequency, value = list of nums
    buckets = [[] for _ in range(len(nums) + 1)]
    for num, freq in count.items():
        buckets[freq].append(num)
    
    result = []
    for i in range(len(buckets) - 1, 0, -1):
        for num in buckets[i]:
            result.append(num)
            if len(result) == k:
                return result
    return result
```

### Canonical Key Grouping
```python
from collections import defaultdict

def group_anagrams(strs):
    groups = defaultdict(list)
    for s in strs:
        key = tuple(sorted(s))  # canonical form
        groups[key].append(s)
    return list(groups.values())
```

### Intelligent Set Membership
```python
def longest_consecutive(nums):
    num_set = set(nums)
    best = 0
    for num in num_set:
        if num - 1 not in num_set:  # only start from sequence beginnings
            length = 1
            while num + length in num_set:
                length += 1
            best = max(best, length)
    return best
```

---

## Variations

| Variation | Key Idea |
|-----------|----------|
| Complement lookup | Store value, check for `target - current` |
| Frequency counting | `Counter(data)` then process frequencies |
| Grouping | Transform element to canonical key, group by key |
| Existence / dedup | Use `set` for O(1) membership tests |
| Two-pass | First pass: build map. Second pass: query map |

---

## Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Insert | O(1) | O(n) |
| Lookup | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Space | O(n) | O(n) |

Worst case is hash collisions — rare with good hash functions.

---

## Common Traps
- Forgetting to handle **duplicate values** (Two Sum: same index used twice)
- Using `dict` when a `set` suffices (saves memory, cleaner code)
- Not recognizing that **sorting + hash map** can solve grouping problems
- Iterating `O(n)` inside a hash map loop, accidentally making it `O(n²)`
