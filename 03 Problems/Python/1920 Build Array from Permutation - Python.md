---
title: Build Array from Permutation
number: 1920
language: python
pattern: arrays-and-indexing
difficulty: easy
tags: [arrays, indexing]
status: worked-example
---

# 1920 Build Array from Permutation (Python)

Pattern: [[Arrays and Indexing]]

> **Worked example.** A teaching walkthrough for a problem I'm still practicing, not my own solution. Try the problem yourself before reading on.

## What it asks

You get `nums`, a **permutation** of `0` to `n − 1`: every number from 0 up to `n − 1` appears exactly once, in some order. Build a new list `ans` where `ans[i] = nums[nums[i]]`.

## Pattern and trigger

**Array indexing**: using a value as a position.

**Trigger:** the values are guaranteed to be valid indexes. So you can look up `nums[i]`, then use that number to look up again.

## Picture

`nums = [0, 2, 1, 5, 3, 4]`

```
i = 3:  nums[3] = 5   →   nums[5] = 4   →   ans[3] = 4
        (inner lookup)    (outer lookup)
```

The inner lookup happens first; its result becomes the index for the outer lookup.

## Questions before coding

- Can a value be out of range? (No: it's a permutation, so every value is a valid index.)
- New list or change `nums` in place? (A new list is the plain version.)

## Trace table

`nums = [0, 2, 1, 5, 3, 4]`

| i | nums[i] | nums[nums[i]] | ans so far |
|---|---|---|---|
| 0 | 0 | 0 | [0] |
| 1 | 2 | 1 | [0, 1] |
| 2 | 1 | 2 | [0, 1, 2] |
| 3 | 5 | 4 | [0, 1, 2, 4] |
| 4 | 3 | 5 | [0, 1, 2, 4, 5] |
| 5 | 4 | 3 | [0, 1, 2, 4, 5, 3] |

## Solution

```python
class Solution:
    def buildArray(self, nums: list[int]) -> list[int]:
        ans = []
        for i in range(len(nums)):
            ans.append(nums[nums[i]])   # value at i, used as an index
        return ans
```

## Complexity

- **Time:** O(n). One pass, and each `nums[...]` lookup is O(1).
- **Space:** O(n) for the new list.

**Follow-up you may be asked:** do it with O(1) extra space. One trick stores two numbers in each slot (`old + n · new`) and decodes them in a second pass. It's worth reading about after you're comfortable with the basic version.

## Watch-outs

- Reading `nums[nums[i]]` inside-out: `nums[i]` first, then the outer lookup.
- `for x in nums` gives values, not positions. Here you want positions, so loop over `range(len(nums))`.
