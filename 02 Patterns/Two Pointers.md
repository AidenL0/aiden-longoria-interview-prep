---
title: Two Pointers
tags: [pattern, two-pointers]
---

# Two Pointers

Back to [[Start Here]]

## What it is

Two index variables move through the same array or string. The common shape: one starts at the left end, one at the right end, and they walk toward each other (**converging pointers**). Another shape has a slow and a fast pointer moving the same direction.

## Why it matters

It often replaces an extra copy of the data (O(n) space) with two numbers (O(1) space), and it's what "do it in place" usually means.

## Triggers

- "Reverse," "palindrome," "compare the start with the end."
- "In place" or "O(1) extra space."
- A **sorted** array and "find a pair": move the left pointer to grow the sum, the right to shrink it.

## Template

```python
left, right = 0, len(items) - 1
while left < right:
    # compare or swap items[left] and items[right]
    left += 1
    right -= 1
```

## Problems

- [[344 Reverse String - CSharp]]: swap from both ends
- [[125 Valid Palindrome - CSharp]]: compare from both ends, skipping what doesn't count

## Watch-outs

- `left < right` versus `left <= right`: decide whether the middle element needs handling.
- Inner "skip" loops need their own `left < right` check, or a pointer can run off the end.
