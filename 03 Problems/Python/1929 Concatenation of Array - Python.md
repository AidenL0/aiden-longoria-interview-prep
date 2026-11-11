---
title: Concatenation of Array
number: 1929
language: python
pattern: arrays-and-indexing
difficulty: easy
tags: [arrays]
status: worked-example
---

# 1929 Concatenation of Array (Python)

Pattern: [[Arrays and Indexing]]

> **Worked example.** A teaching walkthrough for a problem I'm still practicing, not my own solution. Try the problem yourself before reading on.

## What it asks

You get a list `nums` of length `n`. Return a list of length `2n` that is `nums` followed by `nums` again. **Concatenation** means joining end to end.

## Pattern and trigger

**Array building**: the warm-up for every problem that creates a new list.

**Trigger:** the output is the input, rearranged or repeated, with no decisions to make.

## Picture

```
nums: [1, 2, 1]
ans:  [1, 2, 1, 1, 2, 1]
       └ first ┘└ second┘
```

Another way to see it: `ans[i] = nums[i]` and `ans[i + n] = nums[i]`.

## Questions before coding

- Can `nums` be empty? (No: at least one element.)
- New list, or change `nums`? (New list.)

## Trace table

`nums = [1, 2, 1]`, `n = 3`, using the two-slot version.

| i | nums[i] | writes | ans |
|---|---|---|---|
| start | — | `ans = [0] * 6` | [0, 0, 0, 0, 0, 0] |
| 0 | 1 | ans[0], ans[3] | [1, 0, 0, 1, 0, 0] |
| 1 | 2 | ans[1], ans[4] | [1, 2, 0, 1, 2, 0] |
| 2 | 1 | ans[2], ans[5] | [1, 2, 1, 1, 2, 1] |

## Solution

```python
class Solution:
    def getConcatenation(self, nums: list[int]) -> list[int]:
        n = len(nums)
        ans = [0] * (2 * n)        # a list of 2n zeros to fill in

        for i in range(n):
            ans[i] = nums[i]       # first copy
            ans[i + n] = nums[i]   # second copy, n places later

        return ans
```

Python also allows `return nums + nums`. It does the same O(n) work in one line; the loop shows what that line does.

## Complexity

- **Time:** O(n). Each element is copied twice.
- **Space:** O(n) for the new list.

## Watch-outs

- `nums * 2` and `nums + nums` are fine. `ans = nums` followed by `ans.extend(nums)` is not: `ans = nums` makes a second name for the **same** list, so `nums` changes too.
