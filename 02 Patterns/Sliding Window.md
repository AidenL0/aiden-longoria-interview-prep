---
title: Sliding Window
tags: [pattern, sliding-window]
---

# Sliding Window

Back to [[Start Here]]

## What it is

A **window** is a stretch of neighboring items, from index `left` to index `right`. It **slides**: `right` moves forward to take in new items, and `left` moves forward to drop old ones. You keep a running summary of what's inside (a sum, a set, counts) and update it by one item at a time instead of recomputing.

## Why it matters

Checking every stretch separately is O(n²) or worse. A window touches each item at most twice (once entering, once leaving), so it's O(n), even though the code has a loop inside a loop.

## Triggers

- "Longest" or "shortest" stretch (subarray, substring) where some rule holds.
- "Within distance k," "in the last k items."
- Every stretch of a fixed length `x`.

## Template

```python
left = 0
for right in range(len(items)):
    add(items[right])                 # grow
    while window_breaks_the_rule():
        remove(items[left])           # shrink
        left += 1
    record_answer(right - left + 1)   # the window is valid here
```

For a fixed length, add `items[i]` and remove `items[i - x]` each step.

## Problems

- [[219 Contains Duplicate II - CSharp]]: a set of the last k items
- [[904 Fruit Into Baskets - CSharp]]: counting distinct values in the window
- [[2747 Count Zero Request Servers - CSharp]]: a window over sorted events
- [[3505 Minimum Operations to Make Elements Within K Subarrays Equal - Python]]: a fixed window with a running median

## Watch-outs

- The window only works when moving forward never needs undoing. If the data isn't in order, sort it first.
- When counting distinct values, delete a key when its count hits zero, or the distinct count is wrong.
- Window length is `right - left + 1`.
