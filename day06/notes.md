# Day 06 — Linked List

## The Pattern

**What it does**: Manipulates nodes connected by pointers — rewire links to reverse, merge, split, or detect structure.
**When to reach for it**: The problem explicitly involves a linked list, or you need O(1) insert/delete at known positions.

---

## Recognition Triggers

- "Reverse a linked list" → three-pointer swap
- "Merge two sorted lists" → dummy node + comparison
- "Detect a cycle" → Floyd's fast/slow
- "Find the middle" → fast/slow (fast goes 2x)
- "Reorder list" → find middle + reverse + merge
- "Remove nth from end" → two pointers with n-gap

---

## The Template

### Reverse Linked List
```python
def reverse(head):
    prev = None
    curr = head
    while curr:
        nxt = curr.next   # save next
        curr.next = prev  # reverse link
        prev = curr       # advance prev
        curr = nxt        # advance curr
    return prev           # new head
```

**Mnemonic**: Save → Reverse → Advance → Advance. Every step has exactly 4 lines.

### Merge Two Sorted Lists
```python
def merge(l1, l2):
    dummy = ListNode(0)
    curr = dummy
    while l1 and l2:
        if l1.val <= l2.val:
            curr.next = l1
            l1 = l1.next
        else:
            curr.next = l2
            l2 = l2.next
        curr = curr.next
    curr.next = l1 or l2  # attach remainder
    return dummy.next
```

**Key trick**: The **dummy node** eliminates edge cases around which list starts first.

### Detect Cycle (Floyd's Algorithm)
```python
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

### Find Cycle Start
```python
def detect_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            # Phase 2: find entry point
            slow = head
            while slow != fast:
                slow = slow.next
                fast = fast.next
            return slow
    return None
```

### Find Middle
```python
def middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow  # middle (right-middle if even length)
```

### Reorder List (combines all sub-patterns)
```python
def reorder(head):
    # 1. Find middle
    slow = fast = head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next

    # 2. Reverse second half
    second = reverse(slow.next)
    slow.next = None

    # 3. Merge alternating
    first = head
    while second:
        tmp1, tmp2 = first.next, second.next
        first.next = second
        second.next = tmp1
        first, second = tmp1, tmp2
```

---

## Sub-Patterns

| Sub-Pattern | Technique |
|-------------|-----------|
| **Reverse** | prev/curr/nxt three-pointer loop |
| **Merge** | Dummy node + comparison walk |
| **Cycle detect** | Floyd's slow (1x) / fast (2x) |
| **Find middle** | Slow/fast — slow stops at middle |
| **Nth from end** | Two pointers with N-node gap |
| **Composite** | Combine above (e.g., reorder = middle + reverse + merge) |

---

## Common Traps
- Losing the `next` reference before rewiring → always save `nxt = curr.next` first
- Forgetting the **dummy node** → leads to messy head-pointer edge cases
- Off-by-one in **find middle** — `fast.next and fast.next.next` vs `fast and fast.next` gives left-middle vs right-middle
- Not **cutting the list** after finding middle (`slow.next = None`) → creates a cycle
