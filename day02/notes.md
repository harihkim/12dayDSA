# Day 02 — Prefix Sum

## The Pattern

**What it does**: Precomputes cumulative values so any range query becomes O(1).
**When to reach for it**: You need sums (or products) of subarrays repeatedly without re-computing.

---

## Recognition Triggers

- "Sum of subarray from i to j" → prefix sum
- "Product of all elements except self" → prefix × suffix
- "Number of subarrays summing to K" → prefix sum + hash map
- "Balance of 0s and 1s" → transform to +1/−1, then prefix sum
- "Immutable range query" → precompute prefix

---

## The Template

### Basic Prefix Sum
```python
def build_prefix(nums):
    prefix = [0] * (len(nums) + 1)
    for i in range(len(nums)):
        prefix[i + 1] = prefix[i] + nums[i]
    return prefix

# Sum from index l to r (inclusive):
# prefix[r + 1] - prefix[l]
```

**Why it works**: `prefix[i]` stores the sum of `nums[0..i-1]`. Subtracting two prefix values gives any range sum in O(1).

### Prefix × Suffix (Product Except Self)
```python
def product_except_self(nums):
    n = len(nums)
    result = [1] * n

    left = 1
    for i in range(n):
        result[i] = left
        left *= nums[i]

    right = 1
    for i in range(n - 1, -1, -1):
        result[i] *= right
        right *= nums[i]

    return result
```

### Prefix Sum + Hash Map (Subarray Sum = K)
```python
def subarray_sum(nums, k):
    count = 0
    prefix = 0
    seen = {0: 1}  # prefix_sum → how many times we've seen it

    for num in nums:
        prefix += num
        if prefix - k in seen:
            count += seen[prefix - k]
        seen[prefix] = seen.get(prefix, 0) + 1

    return count
```

**Why it works**: If `prefix[j] - prefix[i] == k`, then the subarray `nums[i+1..j]` sums to k. We use a hash map to count how many previous prefix sums equal `current - k`.

### Transform + Prefix Sum (Contiguous Array)
```python
def find_max_length(nums):
    # Transform: 0 → -1, so equal 0s and 1s means sum = 0
    prefix = 0
    first_seen = {0: -1}
    max_len = 0

    for i, num in enumerate(nums):
        prefix += 1 if num == 1 else -1
        if prefix in first_seen:
            max_len = max(max_len, i - first_seen[prefix])
        else:
            first_seen[prefix] = i

    return max_len
```

---

## Variations

| Variation | Key Idea |
|-----------|----------|
| Basic range sum | `prefix[r+1] - prefix[l]` |
| Prefix × suffix | Two passes: left products, right products |
| Prefix sum + hash map | Count subarrays with target sum |
| Transform + prefix | Convert problem to prefix sum (e.g., 0→−1) |
| 2D prefix sum | `prefix[i][j]` for rectangle sums |

---

## Complexity

| | Time | Space |
|-|------|-------|
| Build prefix | O(n) | O(n) |
| Range query | O(1) | — |
| Subarray sum = K | O(n) | O(n) |

---

## Common Traps
- **Off-by-one**: `prefix` array has length `n+1`, with `prefix[0] = 0`
- Forgetting to **initialize `{0: 1}`** in prefix sum + hash map problems
- Not recognizing the **transform trick** (0→−1 for binary arrays)
- Trying to use prefix sum when the array is **modified** (use segment tree instead)
