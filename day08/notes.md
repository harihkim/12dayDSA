# Day 08 — Binary Search

## The Pattern

**What it does**: Halves the search space each step by exploiting sorted/monotonic structure.
**When to reach for it**: Data is sorted, or the answer space is monotonic (if X works, all values > X also work).

---

## Recognition Triggers

- "Find target in sorted array" → classic binary search
- "Search in rotated sorted array" → BS with pivot logic
- "Find minimum speed / capacity / distance" → BS on answer
- "Minimize the maximum" or "maximize the minimum" → BS on answer
- "First/last occurrence" → BS with boundary tracking
- "Koko eating bananas / ship packages" → BS on answer

---

## The Template

### Classic Binary Search
```python
def binary_search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

### Find Left Boundary (First Occurrence)
```python
def find_left(nums, target):
    lo, hi = 0, len(nums) - 1
    result = -1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            result = mid
            hi = mid - 1     # keep searching left
        elif nums[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return result
```

### Binary Search on Answer
```python
import math

def min_eating_speed(piles, h):
    def can_finish(speed):
        return sum(math.ceil(p / speed) for p in piles) <= h

    lo, hi = 1, max(piles)
    while lo < hi:
        mid = (lo + hi) // 2
        if can_finish(mid):
            hi = mid        # mid works, try smaller
        else:
            lo = mid + 1    # mid too slow
    return lo
```

**Why it works**: The answer space is monotonic — if speed X finishes in time, all speeds > X also finish. So we binary search for the minimum X where `can_finish(X)` is True.

### Search in Rotated Sorted Array
```python
def search_rotated(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            return mid

        if nums[lo] <= nums[mid]:       # left half is sorted
            if nums[lo] <= target < nums[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
        else:                            # right half is sorted
            if nums[mid] < target <= nums[hi]:
                lo = mid + 1
            else:
                hi = mid - 1
    return -1
```

### Find Minimum in Rotated Array
```python
def find_min(nums):
    lo, hi = 0, len(nums) - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if nums[mid] > nums[hi]:
            lo = mid + 1    # min is in right half
        else:
            hi = mid        # min is in left half (including mid)
    return nums[lo]
```

---

## BS on Answer — The Meta-Pattern

```
1. Define the answer space: lo = minimum possible, hi = maximum possible
2. Write a feasibility function: can_do(mid) → bool
3. Binary search for the boundary:
   - Minimize answer: if can_do(mid) → hi = mid, else lo = mid + 1
   - Maximize answer: if can_do(mid) → lo = mid, else hi = mid - 1
```

| Problem | Answer Space | Feasibility Check |
|---------|-------------|-------------------|
| Koko Bananas | [1, max(piles)] | Can eat all in h hours at speed mid? |
| Ship Packages | [max(weights), sum(weights)] | Can ship all in d days with capacity mid? |
| Split Array | [max(nums), sum(nums)] | Can split into m subarrays with max sum ≤ mid? |

---

## Template Variants

| Variant | Loop | Update | Returns |
|---------|------|--------|---------|
| Find exact | `lo <= hi` | `lo = mid+1` / `hi = mid-1` | `mid` when found |
| Find left boundary | `lo < hi` | `hi = mid` / `lo = mid+1` | `lo` |
| Find right boundary | `lo < hi` | `lo = mid+1` / `hi = mid` | `hi` (use `mid = (lo+hi+1)//2`) |
| BS on answer (minimize) | `lo < hi` | `hi = mid` / `lo = mid+1` | `lo` |

---

## Common Traps
- **Infinite loop**: `lo < hi` with `lo = mid` when `mid = (lo+hi)//2` — use `mid = (lo+hi+1)//2` instead
- **Off-by-one**: `lo <= hi` vs `lo < hi` changes the semantics completely
- Not recognizing **BS on answer** — the array isn't sorted, but the *answer space* is monotonic
- Rotated array: forgetting to check **which half is sorted** before deciding where target lies
