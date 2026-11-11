---
title: Minimum Operations to Make Elements Within K Subarrays Equal
number: 3505
language: python
pattern: dynamic-programming
difficulty: hard
tags: [sliding-window, median, dynamic-programming]
status: worked-example
---

# 3505 Minimum Operations to Make Elements Within K Subarrays Equal (Python)

Patterns: [[Sliding Window]], [[Dynamic Programming]]

> **Worked example.** A teaching walkthrough for a problem I'm still practicing, not my own solution. Try the problem yourself before reading on.

A hard problem. It's here because it's built from two ideas you can learn separately: the cost of one window, then choosing the best windows.

## What it asks

You get a list `nums` and two numbers `x` and `k`. One **operation** adds 1 to or subtracts 1 from any element. Return the fewest operations so that the list contains at least `k` **non-overlapping subarrays** of length exactly `x` in which every element is equal. (A subarray is a stretch of neighboring elements; non-overlapping means no element is in two of them.)

## Pattern and trigger

1. **Sliding window with a median.** To make a group of numbers equal with the fewest ±1 steps, move them all to their **median** (the middle value when sorted). Any other target is farther from more of the numbers. So each length-`x` window has a cost: the total distance from its numbers to its median.
2. **Dynamic programming** (DP: build the answer for bigger inputs from saved answers to smaller ones). Choose `k` windows that don't overlap and have the smallest total cost.

**Trigger:** "make equal with ±1 steps" → median. "Pick k non-overlapping pieces" → DP over positions.

## Picture

`nums = [5, -2, 1, 3, 7, 3, 6, 4, -1]`, `x = 3`, `k = 2`

```
window [-2, 1, 3]: median 1, cost 3 + 0 + 2 = 5
window [ 3, 6, 4]: median 4, cost 1 + 2 + 0 = 3
                                     total = 8 (the answer)
```

## Questions before coding

- Do windows have to be exactly `x` long? (Yes.)
- Can numbers be negative? (Yes. Distances still work.)
- How big can the input be? (Up to 100,000 elements, so re-sorting every window, O(n · x log x), is too slow. The cost must come from running sums.)

## Trace table

**Part 1: window costs** for the example (`x = 3`, so the median is the middle value).

| start | window | sorted | median | cost |
|---|---|---|---|---|
| 0 | 5, −2, 1 | −2, 1, 5 | 1 | 3 + 0 + 4 = 7 |
| 1 | −2, 1, 3 | −2, 1, 3 | 1 | 3 + 0 + 2 = 5 |
| 2 | 1, 3, 7 | 1, 3, 7 | 3 | 2 + 0 + 4 = 6 |
| 3 | 3, 7, 3 | 3, 3, 7 | 3 | 0 + 0 + 4 = 4 |
| 4 | 7, 3, 6 | 3, 6, 7 | 6 | 3 + 0 + 1 = 4 |
| 5 | 3, 6, 4 | 3, 4, 6 | 4 | 1 + 0 + 2 = 3 |
| 6 | 6, 4, −1 | −1, 4, 6 | 4 | 5 + 0 + 2 = 7 |

**Part 2: choosing windows.** The best pair that doesn't overlap is start 1 (cost 5) and start 5 (cost 3) = 8. Start 5 with start 3 or 4 would overlap.

## Solution

**The cost trick.** Keep each window split into a **low half** (the smaller numbers, ending at the median) and a **high half**, each with a running sum. Then the cost needs no loop:

```
cost = (median · size of low − sum of low) + (sum of high − median · size of high)
```

```python
from sortedcontainers import SortedList   # a list that stays sorted; LeetCode includes it


class SlidingMedianWindow:
    """The current window, split into a low half and a high half."""

    def __init__(self, low_size):
        self.low = SortedList()    # the smallest low_size numbers; its last one is the median
        self.high = SortedList()   # everything bigger
        self.low_size = low_size
        self.low_sum = 0
        self.high_sum = 0

    def add(self, value):
        if self.low and value > self.low[-1]:
            self.high.add(value)
            self.high_sum += value
        else:
            self.low.add(value)
            self.low_sum += value
        self._balance()

    def remove(self, value):
        if value <= self.low[-1]:
            self.low.remove(value)
            self.low_sum -= value
        else:
            self.high.remove(value)
            self.high_sum -= value
        self._balance()

    def _balance(self):
        # keep low at exactly low_size numbers by moving one number at a time
        while len(self.low) > self.low_size:
            moved = self.low.pop()        # biggest number in low
            self.low_sum -= moved
            self.high.add(moved)
            self.high_sum += moved
        while len(self.low) < self.low_size and self.high:
            moved = self.high.pop(0)      # smallest number in high
            self.high_sum -= moved
            self.low.add(moved)
            self.low_sum += moved

    def cost(self):
        median = self.low[-1]
        below = median * len(self.low) - self.low_sum
        above = self.high_sum - median * len(self.high)
        return below + above


class Solution:
    def minOperations(self, nums: list[int], x: int, k: int) -> int:
        n = len(nums)

        # Part 1: cost[start] for every window of length x
        window = SlidingMedianWindow(low_size=(x + 1) // 2)
        cost = []
        for i in range(n):
            window.add(nums[i])
            if i >= x:
                window.remove(nums[i - x])    # slide: drop the number that left
            if i >= x - 1:
                cost.append(window.cost())    # window nums[i-x+1 .. i] is full

        # Part 2: best[j][i] = fewest operations for j windows using only nums[0:i]
        INF = float("inf")
        best = [[INF] * (n + 1) for _ in range(k + 1)]
        for i in range(n + 1):
            best[0][i] = 0                    # zero windows cost nothing

        for j in range(1, k + 1):
            for i in range(1, n + 1):
                skip = best[j][i - 1]         # element i-1 is in no window
                take = INF
                if i >= x:                    # a window ends at element i-1
                    take = best[j - 1][i - x] + cost[i - x]
                best[j][i] = min(skip, take)

        return best[k][n]
```

## Complexity

- **Time:** O(n log x + n · k). Each slide is a few sorted-list operations; the DP fills a `k × n` table.
- **Space:** O(n · k) for the table (it can be cut to two rows).

## Watch-outs

- **Median, not mean.** The average minimizes squared distance, not total distance.
- Helpers indented inside the method and never called: they define functions but run nothing.
- `skip = best[...]`, not `skip - best[...]`. A typo like that is still valid Python, so it fails silently.
- A method whose body is only a comment is an `IndentationError`. Put `pass` or real code there.
