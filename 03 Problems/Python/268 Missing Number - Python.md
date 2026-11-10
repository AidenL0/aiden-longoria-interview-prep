---
title: Missing Number
number: 268
language: python
pattern: simulation-and-math
difficulty: easy
tags: [arrays, math]
---

# 268 Missing Number (Python)

Pattern: [[Simulation and Math]]

## What it asks

You get a list of `n` different numbers taken from the range `0` to `n`. That range holds `n + 1` numbers, so exactly one is missing. Return it.

## Pattern and trigger

**Math**: compare what the total should be with what it is.

**Trigger:** "every number from a known range, except one." The numbers `0` to `n` always add up to `n · (n + 1) / 2`. Subtract the actual total and the gap is the missing number.

## Picture

`nums = [3, 0, 1]`, so `n = 3` and the range is 0, 1, 2, 3.

```
should be: 0 + 1 + 2 + 3 = 6
actually:  3 + 0 + 1     = 4
missing:   6 − 4         = 2
```

## Questions before coding

- Are the numbers all different? (Yes.)
- Is the list sorted? (No. The math method doesn't need it to be.)
- Could the missing number be `n` itself? (Yes: `[0, 1]` is missing 2.)

## Trace table

`nums = [9, 6, 4, 2, 3, 5, 7, 0, 1]`, so `n = 9` and `expected = 9 · 10 / 2 = 45`.

| x | actual after adding x |
|---|---|
| 9 | 9 |
| 6 | 15 |
| 4 | 19 |
| 2 | 21 |
| 3 | 24 |
| 5 | 29 |
| 7 | 36 |
| 0 | 36 |
| 1 | 37 |

`45 − 37 = 8`, so 8 is missing.

## Solution

```python
class Solution:
    def missingNumber(self, nums: list[int]) -> int:
        n = len(nums)
        expected = n * (n + 1) // 2   # sum of 0..n; // is whole-number division

        actual = 0
        for x in nums:                # an explicit loop instead of sum(nums)
            actual += x

        return expected - actual
```

`sum(nums)` would do the same work. It still touches every element, so it's O(n) either way.

## Complexity

- **Time:** O(n). One pass to add everything up.
- **Space:** O(1). Two number variables, no matter how long the list is.

**Other ways:** a set of the numbers, then check 0 to n for the one not in it (O(n) time, O(n) space); or sort and look for the first gap (O(n log n)).

## Watch-outs

- Using `n = len(nums) - 1`. The range goes up to `len(nums)`, because one number is missing.
- In languages with fixed-size integers (like C#), a huge `n` could overflow the sum. Python's integers don't overflow.
