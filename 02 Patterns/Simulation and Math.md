---
title: Simulation and Math
tags: [pattern, simulation, math]
---

# Simulation and Math

Back to [[Start Here]]

## What it is

Two related kinds of easy problems:
- **Simulation:** do exactly what the rules say, step by step. The challenge is getting the rules and their order right.
- **Math shortcuts:** a formula or observation replaces work. The sum of 0 to n is `n · (n + 1) / 2`; two rectangles overlap unless one is entirely to one side of the other.

## Why it matters

These are common warm-ups and first questions in online assessments. They test careful reading, clean conditions and boolean logic more than algorithms.

## Triggers

- A list of rules to apply to each item ("if divisible by 3...").
- "Every number from a known range, except one."
- Geometry with coordinates; "only increase" or "only decrease" moves.
- When listing the ways something *can* happen is messy, list the ways it *can't* and flip the answer.

## Tools

- **Modulo** `%` gives the remainder: `x % 3 == 0` means divisible by 3.
- **Integer division:** `//` in Python. In C#, `/` between two ints already drops the remainder.
- **Boolean logic:** "any one is enough" is `or` / `||`; "all must hold" is `and` / `&&`.

## Problems

- [[412 Fizz Buzz - CSharp]]: rule order with modulo
- [[268 Missing Number - Python]]: expected sum minus actual sum
- [[836 Rectangle Overlap - CSharp]]: check the opposite case
- [[Min Moves to Make Elements Equal - CSharp]]: everyone meets at the max

## Watch-outs

- Put the most specific rule first (15 is both "Fizz" and "Buzz").
- Say "any" or "all" out loud before choosing `||` or `&&`.
- Fixed-size integers (C# `int`) can overflow on large sums; use `long`.
