---
title: Find the Distance Value Between Two Arrays
number: 1385
language: csharp
pattern: binary-search
difficulty: easy
tags: [arrays, binary-search, sorting]
status: worked-example
---

# 1385 Find the Distance Value Between Two Arrays (C#)

Pattern: [[Binary Search]]

> **Worked example.** A teaching walkthrough for a problem I'm still practicing, not my own solution. Try the problem yourself before reading on.

## What it asks

You get two arrays, `arr1` and `arr2`, and a number `d`. Count how many numbers in `arr1` are **far** from every number in `arr2`, where "far" means the distance `|a − b|` is greater than `d`.

## Pattern and trigger

Start with **brute force** (check every pair). The follow-up uses **binary search**: sort `arr2`, then for each `a` find the first number that is at least `a − d` and check whether it's also at most `a + d`.

**Trigger for the follow-up:** "is anything within a range?" on data you can sort.

## Picture

A number line with a window of width `d` on each side of `a`:

```
a = 5, d = 2:     3 ── 4 ── [5] ── 6 ── 7
                  └──── too close ────┘
arr2 = { 10, 9, 1, 8 }: nothing lands in 3..7 → 5 counts
```

`d` is a fixed cutoff given as input. A distance exactly equal to `d` is **close** (it doesn't count), because "far" means greater than `d`.

## Questions before coding

- Is `d` a number I compute, or given? (Given: it's the cutoff.)
- Does a distance equal to `d` count as far? (No: `|a − b| <= d` means close.)
- Negative numbers? (Yes; `Math.Abs` handles them.)

## Trace table

`arr1 = { 4, 5, 8 }`, `arr2 = { 10, 9, 1, 8 }`, `d = 2`

| a | distances to 10, 9, 1, 8 | any ≤ 2? | counts? | count |
|---|---|---|---|---|
| 4 | 6, 5, 3, 4 | no | yes | 1 |
| 5 | 5, 4, 4, 3 | no | yes | 2 |
| 8 | 2, 1, 7, 0 | yes (stop at 10) | no | 2 |

## Solution

**Brute force** (clear, and fine for the input sizes here):

```csharp
public class Solution {
    public int FindTheDistanceValue(int[] arr1, int[] arr2, int d) {
        int count = 0;

        foreach (int a in arr1) {
            bool far = true;                  // assume far until proven close
            foreach (int b in arr2) {
                if (Math.Abs(a - b) <= d) {   // close: this a doesn't count
                    far = false;
                    break;                    // no need to check the rest
                }
            }
            if (far) count++;
        }
        return count;
    }
}
```

**Follow-up with binary search** (`lo` and `hi` close in on the first number ≥ `a − d`):

```csharp
Array.Sort(arr2);
int count = 0;
foreach (int a in arr1) {
    int lo = 0, hi = arr2.Length;          // hi = Length means "none found"
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr2[mid] < a - d) lo = mid + 1;
        else hi = mid;
    }
    // far if nothing is >= a - d, or the first such number is past a + d
    if (lo == arr2.Length || arr2[lo] > a + d) count++;
}
return count;
```

## Complexity

- **Brute force:** O(n · m) time, O(1) space.
- **Binary search:** O((n + m) log m) time: sorting `arr2`, then one search per `a`.

## Watch-outs

- `<` versus `<=`: "far" is `> d`, so "close" is `<= d`.
- Setting `far = true` inside the inner loop resets it every pass. Set it once per `a`, before the inner loop.
