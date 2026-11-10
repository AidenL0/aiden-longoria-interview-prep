---
title: Python Comprehensions
tags: [fundamentals, python, syntax]
---

# Python Comprehensions

Related: [[Complexity]] · [[Python Roadmap]]

## What it is

A **comprehension** is a short way to build a list, set or dictionary from a loop. It's a `for` loop with the body moved to the front:

```python
squares = []
for x in nums:                      squares = [x * x for x in nums]
    squares.append(x * x)
```

What used to go inside `append(...)` moves before the `for`. Everything else reads in loop order.

## Why it matters

Solutions and editorials (official write-ups) use comprehensions everywhere. You need to read them fluently even if you write explicit loops yourself.

## The three slots

```
[ expression     for item in iterable     if condition ]
     ↑                  ↑                        ↑
 what goes in      names each item          optional filter
   the list
```

The expression slot is never empty, and it isn't the same thing as the loop variable. In `[w.upper() for w in words]`, `w` names each item and `w.upper()` decides what gets stored.

## The four forms

```python
[x * x for x in nums]          # list
{len(w) for w in words}        # set: duplicates removed automatically
{w: len(w) for w in words}     # dictionary: the expression is key: value (colon, not comma)
(x * x for x in nums)          # generator: lazy, builds nothing until you loop over it
```

## `enumerate`: index and value together

```python
for i, x in enumerate(nums):   # nums = [3, 1, 4] gives (0, 3), (1, 1), (2, 4)
```

The first name always gets the **index**, whatever you call it. `for value, index in enumerate(nums)` puts the index in `value`. Name the index something that reads like one (`i`, `idx`).

| Need | Use |
|---|---|
| Values only | `for x in nums` |
| Values and positions | `for i, x in enumerate(nums)` |
| Index math, custom steps, going backward | `for i in range(...)` |

## Watch-out: the last duplicate wins

```python
nums = [3, 1, 4, 1, 5]
{x: i for i, x in enumerate(nums)}   # {3: 0, 1: 3, 4: 2, 5: 4}
```

`1` appears at index 1 and index 3. A dictionary keeps one entry per key, so the later write survives. If you need the **first** position, use a loop:

```python
first = {}
for i, x in enumerate(nums):
    if x not in first:
        first[x] = i
```

## Complexity still applies

One line is not one operation. Cost = (number of items) × (cost of the expression + cost of the filter).

| Code | Time |
|---|---|
| `[len(w) for w in words]` | O(n) |
| `[x for x in a if x in b]` with `b` a **list** | O(n · m): `in` scans `b` every time |
| `[x for x in a if x in b]` with `b` a **set** | O(n): convert `b` to a set first |
| `[sorted(w) for w in words]` | O(n · k log k) |

## Drills: cover the right column

| In words | Comprehension |
|---|---|
| the length of every word | `[len(w) for w in words]` |
| uppercase versions of words shorter than 5 letters | `[w.upper() for w in words if len(w) < 5]` |
| the different word lengths | `{len(w) for w in words}` |
| each value mapped to its (last) index | `{x: i for i, x in enumerate(nums)}` |
| each value mapped to its **first** index | not a comprehension: a loop with `if x not in d` |

Reading the template isn't enough; typing it is what makes it stick. Write five from scratch without looking.
