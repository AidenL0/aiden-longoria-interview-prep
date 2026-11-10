---
title: Binary Search
tags: [pattern, binary-search]
---

# Binary Search

Back to [[Start Here]]

## What it is

On **sorted** data, look at the middle item. If it's too small, the answer must be in the right half; if too big, in the left half. Throw away the wrong half and repeat. Each step halves what's left, so it takes O(log n) steps: about 17 for 100,000 items.

## Why it matters

It turns "check every item" (O(n)) into about log n checks, and it's a favorite for follow-up questions like "can you do better than O(n · m)?"

## Triggers

- The data is sorted, or you're allowed to sort it.
- "Find the first item that is at least x," "is anything in this range?"
- An answer that's a number where "too small" and "big enough" are easy to test (binary search on the answer).

## Templates

**Find an exact value:**

```python
lo, hi = 0, len(arr) - 1
while lo <= hi:
    mid = lo + (hi - lo) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        lo = mid + 1
    else:
        hi = mid - 1
return -1
```

**Find the first item ≥ target** (returns `len(arr)` if there is none):

```python
lo, hi = 0, len(arr)
while lo < hi:
    mid = lo + (hi - lo) // 2
    if arr[mid] < target:
        lo = mid + 1
    else:
        hi = mid
return lo
```

Python's `bisect.bisect_left(arr, target)` does the second one for you.

## Problems

- [[1385 Find the Distance Value Between Two Arrays - CSharp]]: the follow-up uses "first item ≥ a − d"

## Watch-outs

- The two templates use different loop conditions (`<=` versus `<`) and different updates for `hi`. Mixing them causes infinite loops or skipped items.
- `lo + (hi - lo) / 2` instead of `(lo + hi) / 2` avoids overflow in languages with fixed-size integers like C#.
- Binary search on unsorted data gives wrong answers without any error.
