---
title: Sorting
tags: [pattern, sorting]
---

# Sorting

Back to [[Start Here]]

## What it is

Putting items in order. Built-in sorts (`sorted()` and `list.sort()` in Python, `Array.Sort` and `List.Sort` in C#) take O(n log n) time. Sorting is rarely the whole answer; it's the step that makes another pattern possible.

## Why it matters

Once data is sorted, equal items sit next to each other, binary search works, and pointers or windows can move in one direction without going back.

## Triggers

- "Group," "find duplicates," "closest pair": sort, then compare neighbors.
- Two arrays that belong together (parallel arrays): sort one and carry the other along.
- Many questions about ranges or times: sort both the data and the questions, then sweep once.

## Templates

**Sort by a key, carrying a partner:**

```python
pairs = sorted(zip(heights, names))           # Python: sorts by height, names come along
```

```csharp
Array.Sort(heights, names);                    // C#: sorts both arrays in place
```

**Process in sorted order, answer in original order:** carry each item's original index as the partner, then write each answer to `answer[original_index]`.

## Problems

- [[2418 Sort the People - CSharp]]: sort one array, carry the other
- [[2747 Count Zero Request Servers - CSharp]]: sort events and queries, then sweep
- [[49 Group Anagrams - Python]]: sorting letters to build a key
- [[1385 Find the Distance Value Between Two Arrays - CSharp]]: sort so binary search works

## Watch-outs

- Python's `sorted()` returns a new list; `list.sort()` and C#'s `Array.Sort` change the original.
- Sorting hides O(n log n) work. Count it in the complexity.
- `b = a` doesn't copy. Sorting `b` scrambles `a` too.
