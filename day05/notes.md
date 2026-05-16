# Day 05 — Stack

## The Pattern

**What it does**: Maintains a LIFO (last-in, first-out) structure to track nesting, ordering, or history.
**When to reach for it**: You need to match brackets, find the next greater/smaller element, or process nested structures.

---

## Recognition Triggers

- "Valid parentheses / balanced brackets" → stack matching
- "Next greater element" → monotonic decreasing stack
- "Next smaller element" → monotonic increasing stack
- "Daily temperatures / stock span" → monotonic stack with indices
- "Largest rectangle in histogram" → monotonic increasing stack
- "Evaluate reverse polish notation" → operand stack

---

## The Template

### Stack Matching (Valid Parentheses)
```python
def is_valid(s):
    stack = []
    match = {')': '(', '}': '{', ']': '['}
    for char in s:
        if char in match:               # closer
            if not stack or stack[-1] != match[char]:
                return False
            stack.pop()
        else:                            # opener
            stack.append(char)
    return len(stack) == 0
```

### Monotonic Stack — Next Greater Element
```python
def daily_temperatures(temps):
    n = len(temps)
    result = [0] * n
    stack = []  # indices of unresolved temps (decreasing order)

    for i in range(n):
        while stack and temps[i] > temps[stack[-1]]:
            prev = stack.pop()
            result[prev] = i - prev
        stack.append(i)

    return result
```

**Why it works**: The stack holds indices of temperatures we haven't found a warmer day for yet. When we find a warmer day, we pop and record the distance. The stack stays in decreasing order of temperature.

### Min Stack (O(1) getMin)
```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val):
        self.stack.append(val)
        min_val = min(val, self.min_stack[-1] if self.min_stack else val)
        self.min_stack.append(min_val)

    def pop(self):
        self.stack.pop()
        self.min_stack.pop()

    def top(self):
        return self.stack[-1]

    def getMin(self):
        return self.min_stack[-1]
```

### Largest Rectangle in Histogram
```python
def largest_rectangle(heights):
    stack = []  # indices, increasing order of height
    max_area = 0
    heights.append(0)  # sentinel to flush remaining

    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)

    heights.pop()  # remove sentinel
    return max_area
```

---

## Monotonic Stack Variants

| Type | Stack Order | Finds |
|------|-------------|-------|
| **Decreasing** | Top is smallest | Next **greater** element |
| **Increasing** | Top is largest | Next **smaller** element |

**Key insight**: You always push the current element. You pop when the current element *breaks* the monotonic order — and the popped element's answer is the current element.

---

## Complexity

| Operation | Time |
|-----------|------|
| Push | O(1) |
| Pop | O(1) |
| Peek | O(1) |
| Monotonic stack (full pass) | O(n) total — each element pushed and popped at most once |

---

## Common Traps
- Checking `stack[-1]` on an **empty stack** → always guard with `if stack`
- Storing **values** instead of **indices** — you almost always want indices for distance calculations
- Getting the monotonic direction **backwards** — think about what you're looking for (greater → decreasing stack)
- Forgetting the **sentinel trick** in histogram — append 0 to flush all remaining bars
