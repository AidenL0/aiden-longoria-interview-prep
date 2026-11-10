---
title: Dynamic Programming
tags: [pattern, dynamic-programming]
---

# Dynamic Programming

Back to [[Start Here]]

## What it is

**Dynamic programming (DP)** solves a problem by solving smaller versions of it first, saving each answer in a table, and building bigger answers from saved ones. It works when the best answer for a big input is made of best answers for smaller inputs, and the same smaller questions come up again and again.

## Why it matters

Without saving answers, many "try every choice" problems take exponential time. With a table, they often become O(n) or O(n · k).

## Triggers

- "Minimum cost," "maximum total," "number of ways."
- A sequence of choices where each choice limits the next ones ("pick k pieces that don't overlap," "you can't take two neighbors").

## How to build one

1. **Define the table in words.** "`best[i]` = the answer using only the first `i` items." Most DP mistakes are a fuzzy definition.
2. **Write the choice at each step.** Usually "use this item" or "skip it," and take the better of the two.
3. **Set the starting values** (base cases), like "zero items cost nothing."
4. **Fill the table in an order** where every value you read has already been computed.
5. **Read the answer** from the right cell.

## Template: skip or take

```python
INF = float("inf")
best = [[INF] * (n + 1) for _ in range(k + 1)]
for i in range(n + 1):
    best[0][i] = 0                       # zero pieces cost nothing
for j in range(1, k + 1):                # j pieces
    for i in range(1, n + 1):            # using the first i items
        skip = best[j][i - 1]
        take = INF
        if i >= size:
            take = best[j - 1][i - size] + cost_of_piece_ending_at(i - 1)
        best[j][i] = min(skip, take)
```

## Problems

- [[3505 Minimum Operations to Make Elements Within K Subarrays Equal - Python]]: choose k non-overlapping windows at the lowest cost

## Watch-outs

- Off-by-one between "first `i` items" and "item at index `i`." Write the definition next to the code.
- Start "impossible" cells at infinity, not 0, when you're taking a minimum.
- Start with the simplest classics before hard ones: Climbing Stairs (70), House Robber (198), Coin Change (322).
