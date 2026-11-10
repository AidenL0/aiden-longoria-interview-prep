---
title: Arrays and Indexing
tags: [pattern, arrays]
---

# Arrays and Indexing

Back to [[Start Here]]

## What it is

An **array** (a Python `list`, a C# `int[]`) stores items in numbered slots starting at 0. Reading or writing any slot by its number is O(1). Many easy problems are about reading the right slots in the right order.

## Why it matters

Almost every other pattern sits on top of this one. Getting indexes right (where a loop starts, where it stops, which index belongs to which array) prevents most early bugs.

## Triggers

- The values are guaranteed to be valid positions ("a permutation of 0 to n − 1").
- The output is the input rearranged, repeated or combined position by position.
- "Common to all": compare several arrays or strings position by position (a **vertical scan**).

## Template

```python
for i in range(len(nums)):      # i = the position; nums[i] = what's in it
    ...                         # read nums[i], nums[i + 1], nums[nums[i]], ...
```

Read each index out loud: "word `j`, letter `i`" for `strs[j][i]`.

## Problems

- [[1920 Build Array from Permutation - Python]]: a value used as an index
- [[1929 Concatenation of Array - Python]]: writing to two positions per step
- [[14 Longest Common Prefix - CSharp]]: vertical scan across words

## Watch-outs

- **Off by one:** a list of length n has indexes 0 to n − 1. `range(n)` stops before n.
- **Index versus value:** `i` is *which slot*; `nums[i]` is *what's in it*.
- **Alias versus copy:** `b = a` gives the same list a second name. Copy with `a[:]` (Python) or `(int[])a.Clone()` (C#).
