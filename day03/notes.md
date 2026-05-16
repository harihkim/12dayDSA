# Day 03 — Two Pointers

## The Pattern

**What it does**: Uses two indices to scan data from both ends or in tandem, eliminating search space efficiently.
**When to reach for it**: The data is sorted (or you can sort it), and you need to find pairs, sections, or validate symmetry.

---

## Recognition Triggers

- "Find pair in sorted array summing to X" → converging pointers
- "Find triplet summing to X" → fix one + converging pair
- "Is this a palindrome?" → converge from both ends
- "Remove duplicates in-place" → read/write pointers
- "Container with most water / trapping rain water" → move the limiting side

---

## The Template

### Converging Pointers (Pair Sum)
```python
def two_sum_sorted(nums, target):
    left, right = 0, len(nums) - 1
    while left < right:
        total = nums[left] + nums[right]
        if total == target:
            return [left, right]
        elif total < target:
            left += 1    # need bigger sum
        else:
            right -= 1   # need smaller sum
    return []
```

**Why it works**: Sorted order guarantees that moving `left` right increases the sum and moving `right` left decreases it. Every step eliminates a row/column of the search space.

### Fix One + Two Pointer (3Sum)
```python
def three_sum(nums):
    nums.sort()
    result = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:
            continue  # skip duplicate anchors
        left, right = i + 1, len(nums) - 1
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            if total == 0:
                result.append([nums[i], nums[left], nums[right]])
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                left += 1
                right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1
    return result
```

### Palindrome Check
```python
def is_palindrome(s):
    s = ''.join(c.lower() for c in s if c.isalnum())
    left, right = 0, len(s) - 1
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True
```

### Container With Most Water
```python
def max_area(height):
    left, right = 0, len(height) - 1
    best = 0
    while left < right:
        area = min(height[left], height[right]) * (right - left)
        best = max(best, area)
        if height[left] < height[right]:
            left += 1    # move the shorter side
        else:
            right -= 1
    return best
```

### Trapping Rain Water
```python
def trap(height):
    left, right = 0, len(height) - 1
    left_max = right_max = 0
    water = 0
    while left < right:
        if height[left] < height[right]:
            left_max = max(left_max, height[left])
            water += left_max - height[left]
            left += 1
        else:
            right_max = max(right_max, height[right])
            water += right_max - height[right]
            right -= 1
    return water
```

---

## Variations

| Variation | Setup |
|-----------|-------|
| **Converging** | `left=0, right=n-1`, move inward |
| **Same direction** | `slow=0, fast=0`, both move right (see Sliding Window) |
| **Read/Write** | `read` scans all, `write` marks valid output position |
| **Fix + scan** | Fix one element, two-pointer scan the rest |

---

## Complexity

| | Time | Space |
|-|------|-------|
| Pair search | O(n) | O(1) |
| 3Sum | O(n²) | O(1) extra |
| Palindrome | O(n) | O(1) |

---

## Common Traps
- Forgetting to **sort first** — two pointers needs sorted data
- Not handling **duplicates** in 3Sum (infinite loops or duplicate results)
- Moving the **wrong pointer** — always move the side that limits the answer
- Confusing two pointers with sliding window — two pointers converge, window expands
