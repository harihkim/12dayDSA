# Day 04 — Sliding Window

## The Pattern

**What it does**: Maintains a moving window over contiguous elements, expanding right and shrinking left.
**When to reach for it**: You need the longest/shortest/best contiguous subarray or substring that satisfies some condition.

---

## Recognition Triggers

- "Longest substring with at most K distinct characters" → variable window
- "Minimum window containing all characters of T" → variable window, shrink when valid
- "Maximum sum subarray of size K" → fixed window
- "Permutation in string / anagram check" → fixed window
- "Longest repeating character replacement" → variable window with frequency tracking

---

## The Template

### Fixed Window (Size K)
```python
def max_sum_k(nums, k):
    window = sum(nums[:k])
    best = window
    for i in range(k, len(nums)):
        window += nums[i] - nums[i - k]  # slide: add right, drop left
        best = max(best, window)
    return best
```

### Variable Window — Longest Valid
```python
def longest_valid_window(s):
    left = 0
    best = 0
    state = {}  # track window state (frequencies, counts, etc.)

    for right in range(len(s)):
        # EXPAND: add s[right] to state
        update_state_add(state, s[right])

        # SHRINK: while window is invalid, remove from left
        while not is_valid(state):
            update_state_remove(state, s[left])
            left += 1

        # UPDATE: window [left..right] is valid
        best = max(best, right - left + 1)

    return best
```

### Longest Substring Without Repeating Characters
```python
def length_of_longest_substring(s):
    char_index = {}
    left = 0
    best = 0

    for right in range(len(s)):
        if s[right] in char_index and char_index[s[right]] >= left:
            left = char_index[s[right]] + 1
        char_index[s[right]] = right
        best = max(best, right - left + 1)

    return best
```

### Minimum Window Substring
```python
from collections import Counter

def min_window(s, t):
    need = Counter(t)
    missing = len(t)
    left = 0
    best = (0, float('inf'))

    for right, char in enumerate(s):
        if need[char] > 0:
            missing -= 1
        need[char] -= 1

        while missing == 0:             # window is valid → try to shrink
            if right - left < best[1] - best[0]:
                best = (left, right)
            need[s[left]] += 1
            if need[s[left]] > 0:
                missing += 1
            left += 1

    return s[best[0]:best[1]+1] if best[1] != float('inf') else ""
```

### Longest Repeating Character Replacement
```python
def character_replacement(s, k):
    count = {}
    left = 0
    max_freq = 0
    best = 0

    for right in range(len(s)):
        count[s[right]] = count.get(s[right], 0) + 1
        max_freq = max(max_freq, count[s[right]])

        # window size - max_freq = chars we need to replace
        while (right - left + 1) - max_freq > k:
            count[s[left]] -= 1
            left += 1

        best = max(best, right - left + 1)

    return best
```

---

## Fixed vs Variable Window

| | Fixed | Variable |
|-|-------|----------|
| **Window size** | Always K | Grows and shrinks |
| **Movement** | Slide right, always drop left | Expand right, shrink left only when needed |
| **Questions** | "Max sum of K elements" | "Longest/shortest with condition" |

---

## Why It's O(n)

Each element enters the window once (right pointer) and leaves at most once (left pointer). Total operations: 2n = O(n). The inner `while` doesn't nest — `left` only moves forward.

---

## Common Traps
- Forgetting to **update state** when shrinking (left pointer moves but you don't update frequency)
- Confusing **longest** (shrink when invalid) vs **shortest** (shrink when valid)
- Off-by-one on window size: `right - left + 1` not `right - left`
- Using fixed window template for variable problems or vice versa
