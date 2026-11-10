---
title: Reading a Problem
tags: [fundamentals, process]
---

# Reading a Problem

Back to [[Start Here]] · Next: [[Big O Notation]]

## What it is

A coding problem is a short description with rules hidden in it. Most early mistakes aren't coding mistakes; they're reading mistakes: a misread boundary, an input you didn't expect, or answering a slightly different question.

## Why it matters

In an interview, the first few minutes of questions and examples are graded too. Interviewers want to see you understand the problem before you type.

## The steps

1. **Restate it in your own words.** One or two sentences: "Given ___, return ___." If you can't, reread.
2. **Name the inputs and outputs, with types.** A list of integers? Can it be empty? Do they want indexes or values? A new list, or change the input in place?
3. **Find the fixed numbers and what they mean.** Some numbers are given as limits, not computed. If a problem says "distance greater than `d`," then `d` is a cutoff you compare against, and you need to decide whether "exactly `d`" counts. Write `<` or `<=` on purpose.
4. **Ask the edge-case questions.** Empty input? One item? Duplicates? Negative numbers? Very large values?
5. **Work one example by hand.** Make a **trace table**: one column per variable, one row per step. If your hand answer doesn't match the expected output, you've misunderstood something, and it's cheaper to find out now.
6. **Read the limits.** If the input can be 100,000 items, an O(n²) approach will be too slow. The limits tell you how fast your answer must be.
7. **Look for the trigger.** Which phrase points at a pattern? "Pair that adds to a target" → hashing. "Longest stretch where..." → sliding window. "Most recent unfinished thing" → stack. See the `02 Patterns` folder.
8. **Say the approach in one sentence before coding,** then its complexity.

## A small example

> Count how many numbers in `arr1` have no number in `arr2` within distance `d`.

- Restated: for each `a` in `arr1`, check that every `b` in `arr2` is more than `d` away; count the ones that pass.
- `d` is given, not computed. A distance exactly equal to `d` is "within" (close).
- Hand test: `arr1 = [4, 5, 8]`, `arr2 = [10, 9, 1, 8]`, `d = 2` → 4 and 5 pass, 8 doesn't (it's 0 from 8). Answer 2.

The worked version is [[1385 Find the Distance Value Between Two Arrays - CSharp]].
