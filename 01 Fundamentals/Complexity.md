---
title: Complexity
tags: [fundamentals, big-o, time, space]
---

# Complexity

Before this: [[Big O Notation]] · Back to [[Start Here]]

## What it is

**Time complexity** counts the operations a program performs. **Space complexity** counts the **extra** memory it uses beyond the input. Both are written in [[Big O Notation]].

## The one question to ask

> **How many elements must this touch?**

Complexity measures the work the machine does, not how long the line of code looks. `"x" * 1_000_000` is a short line that writes a million characters.

So don't just look for `for` keywords. Many built-in functions have a loop hidden inside them.

**Hides a loop (O(n) each):** `sorted()`, `sum()`, `min()`, `max()`, `x in some_list`, `set(arr)`, `Counter(s)`, `"".join(parts)`, `s.replace(...)`, `s[::-1]`, slicing like `arr[a:b]`, `==` on two lists, `list.count(x)`

**Really O(1):** `len()` (the length is stored, not counted), `arr[i]`, `append()`, `pop()` from the end, dictionary and set lookups (`x in some_set`), `heap[0]`

**O(1) to create, cost comes later:** `range()`, `enumerate()`, `zip()`. They're recipes; nothing is built until you loop over them.

An O(n) built-in inside an O(n) loop is the classic accidental O(n²), and it often looks like one innocent line.

## Why `arr[i]` is O(1)

A Python list stores **pointers** (memory addresses), and every pointer is the same size (8 bytes). So the computer finds slot `i` with arithmetic:

```
address of arr[3] = start address + 3 × 8
```

One multiply, one add, one read, no matter how long the list is.

A **linked list** has no such formula: to reach item `i` you follow `i` pointers one by one, so it's O(i). (That's also why `deque[i]` in the middle is slow.)

## Time versus space

| | Counts |
|---|---|
| **Time** | operations performed |
| **Space** | **extra** memory the algorithm allocates |

"Extra" matters: the input is already in memory, so it doesn't count. Otherwise every solution would be at least O(n) space and the measure would tell you nothing.

Neither is "better." It's a trade, and naming the trade is what interviewers listen for. Two Sum with two nested loops is O(n²) time and O(1) space. With a dictionary it's O(n) time and O(n) space: the speed was bought with memory.

**Two space costs that hide:**
- **Recursion** uses one stack frame per active call. Going down a tree of height h costs O(h) space even with no data structures: O(log n) for a balanced tree, O(n) for a tree shaped like a line.
- **Slicing** makes a copy. Inside a loop or recursion, that can quietly turn into O(n²).

## Drills: cover the right column

| Code | Time / Space |
|---|---|
| `s == s[::-1]` | O(n) / O(n): one pass builds the reversed copy, another compares |
| `len(set(nums))` | O(n) / O(n): `len` is free, building the set is the cost |
| `arr[2] = "x" * 1_000_000` | O(n) to build the string, O(1) to store it |
| `sorted(nums)`, then one scan | O(n log n) / O(n) for the sorted copy |
| `"".join(parts)` versus `s += c` in a loop | O(n) versus O(n²) |

The first row is a common slip: no visible loop doesn't mean no loop.
