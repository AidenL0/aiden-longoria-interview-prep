---
title: Hashing
tags: [pattern, hashing, dictionary, set]
---

# Hashing

Back to [[Start Here]]

## What it is

A **hash table** finds a key in about one step, however much it holds. Python's `dict` and `set`, and C#'s `Dictionary` and `HashSet`, are hash tables. A set answers "have I seen this?"; a dictionary also stores something about it (an index, a count, a group).

## Why it matters

It's the most common way to turn an O(n²) "compare everything with everything" solution into O(n): instead of searching again, you remember.

## Triggers

- "Have I seen this before?" → a set.
- "Find a pair that combines to a target" → a dictionary of what you've seen.
- "Count how many times each thing appears" → a dictionary (or an array of 26 for letters).
- "Group things that are the same in some way" → a dictionary from a computed key to a list.
- Fixed symbol values (like Roman numerals) → a lookup table.

## Template

```python
seen = {}                          # or set()
for i, x in enumerate(nums):
    if something_about(x) in seen:  # O(1) check
        ...
    seen[x] = i                    # remember x for later items
```

## Problems

- [[1 Two Sum - Python]] · [[1 Two Sum - CSharp]]: dictionary of seen numbers
- [[217 Contains Duplicate - CSharp]]: seen-before set
- [[242 Valid Anagram - CSharp]]: counting letters
- [[49 Group Anagrams - Python]] · [[49 Group Anagrams - CSharp]]: grouping by a computed key
- [[13 Roman to Integer - CSharp]]: lookup table

## Watch-outs

- Check before you store, or an item can match itself.
- Reading a missing key: Python `d[k]` raises `KeyError`, C# throws `KeyNotFoundException`. Use `d.get(k, 0)`, `in`, `TryGetValue` or `GetValueOrDefault`.
- The speed costs memory: O(n) space. Say so when you give the complexity.
