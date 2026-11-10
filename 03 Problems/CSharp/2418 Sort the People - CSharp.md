---
title: Sort the People
number: 2418
language: csharp
pattern: sorting
difficulty: easy
tags: [sorting, parallel-arrays]
---

# 2418 Sort the People (C#)

Pattern: [[Sorting]] · Next: [[2747 Count Zero Request Servers - CSharp]]

## What it asks

Two **parallel arrays**: `names[i]` and `heights[i]` describe the same person. All heights are different. Return the names ordered from tallest to shortest.

## Pattern and trigger

**Sort one array and carry its partner along.** In C#, `Array.Sort(keys, items)` sorts `keys` and moves each `items[i]` to wherever its key goes, as if each column were glued together.

**Trigger:** two arrays that belong together; order one by the other.

## Picture

```
before:  heights  180   165   170        names  Mary  John  Emma
sort heights, names come along:
after:   heights  165   170   180        names  John  Emma  Mary
reverse names for tallest first:                Mary  Emma  John
```

## Questions before coding

- Which array is the key? (Heights. Swapping the two arguments would sort alphabetically by name.)
- `Array.Sort` sorts smallest first; the problem wants tallest first. (Reverse afterward, or sort with a descending comparison.)
- Does sorting change the original arrays? (Yes. `Array.Sort` works in place.)

## Trace table

| Step | heights | names |
|---|---|---|
| start | 180, 165, 170 | Mary, John, Emma |
| `Array.Sort(heights, names)` | 165, 170, 180 | John, Emma, Mary |
| `Array.Reverse(names)` | — | Mary, Emma, John |

## Solution

```csharp
public class Solution {
    public string[] SortPeople(string[] names, int[] heights) {
        Array.Sort(heights, names);   // sort by height; each name moves with its height
        Array.Reverse(names);         // shortest-first becomes tallest-first
        return names;
    }
}
```

The Python version of the same idea is `sorted(zip(heights, names))`, which builds a new list of pairs instead of changing the arrays.

## Complexity

- **Time:** O(n log n) for the sort.
- **Space:** O(1) extra beyond the sort's own work: both arrays are changed in place.

## Watch-outs

- Argument order: `Array.Sort(keys, items)`. The first array decides the order.
- `int[] copy = heights;` is not a copy, just a second name for the same array. To keep the original, use `(int[])heights.Clone()`.
- This "carry a partner" trick is the base of harder problems: carry each item's **original index** as the partner, and you can process items in sorted order but write answers back to their original slots.
