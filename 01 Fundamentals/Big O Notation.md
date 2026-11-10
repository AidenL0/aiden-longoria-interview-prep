---
title: Big O Notation
tags: [fundamentals, big-o]
---

# Big O Notation

Next: [[Complexity]] · Back to [[Start Here]]

## What it is

**Big O** describes how the work a program does grows as its input grows. It ignores the exact speed of your computer and small details, and keeps only the shape of the growth.

If a list has `n` items:
- reading one item by its position takes the same time whether `n` is 10 or 10 million: **O(1)**, "constant time";
- looking at every item once takes about `n` steps: **O(n)**, "linear time";
- comparing every item with every other item takes about `n × n` steps: **O(n²)**, "quadratic time."

## Why it matters

Interviewers ask "what's the complexity?" after almost every problem. LeetCode inputs often have 100,000 items. An O(n) solution does about 100,000 steps and finishes instantly; an O(n²) one does about 10,000,000,000 and times out.

## The common ones, fastest to slowest

| Big O | Name | 1,000 items → about | Typical example |
|---|---|---|---|
| O(1) | constant | 1 step | `arr[i]`, a dictionary lookup |
| O(log n) | logarithmic | 10 steps | binary search (halving each time) |
| O(n) | linear | 1,000 steps | one loop over the list |
| O(n log n) | linearithmic | 10,000 steps | sorting |
| O(n²) | quadratic | 1,000,000 steps | a loop inside a loop over the same list |
| O(2ⁿ) | exponential | more than atoms in the universe | trying every subset |
| O(n!) | factorial | even more | trying every ordering |

O(n log n) comes **after** O(n): `n log n` is `n` multiplied by something bigger than 1, so it grows faster.

## How to read it

- **Drop constants.** Two separate passes over a list are 2n steps, still O(n).
- **Keep the biggest term.** n² + n is O(n²); once n is large, the n² part is all that matters.
- **Different inputs get different letters.** Comparing every item of a list of size n with every item of a list of size m is O(n · m), not O(n²).

## A quick check

For each line of code, ask: **"How many elements must this touch?"** More on that in [[Complexity]].
