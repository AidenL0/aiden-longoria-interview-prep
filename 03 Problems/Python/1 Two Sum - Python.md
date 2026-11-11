---
title: Two Sum
number: 1
language: python
pattern: hashing
difficulty: easy
tags: [arrays, hashing, dictionary]
status: worked-example
---

# 1 Two Sum (Python)

Other language: [[1 Two Sum - CSharp]] · Pattern: [[Hashing]]

> **Worked example.** I've solved Two Sum in C# ([[1 Two Sum - CSharp]]); this Python version is a walkthrough I haven't written myself yet. Try it yourself before reading on.

## What it asks

You get a list of numbers, `nums`, and a number called `target`. Exactly two numbers in the list add up to `target`. Return their **indexes** (their positions in the list, counting from 0), not the numbers themselves.

## Pattern and trigger

**Hashing**, using a dictionary (Python's `dict`, a lookup table that finds a key in one step).

**Trigger:** "find two things that combine to a target." For each number, the partner it needs is `target - number`. If you remember every number you've already seen, you can check for that partner in one step instead of searching the whole list again.

## Picture

`nums = [4, 11, 7, 2]`, `target = 9`

```
index:   0    1    2    3
value:   4   11    7    2
                   ^    ^
                   7 + 2 = 9  → answer [2, 3]
```

At index 3 the number is 2. It needs 9 − 2 = 7. Have we seen a 7? Yes, at index 2.

## Questions before coding

- Can the same element be used twice? (No: two different indexes.)
- Is there always exactly one answer? (Yes, on LeetCode.)
- Can numbers be negative? (Yes. The method doesn't care.)
- Does the order of the two indexes matter? (Any order is accepted.)

## Trace table

`nums = [4, 11, 7, 2]`, `target = 9`. `seen` maps a number to the index where it appeared.

| i | nums[i] | need = 9 − nums[i] | need in seen? | seen after this step |
|---|---|---|---|---|
| 0 | 4 | 5 | no | {4: 0} |
| 1 | 11 | −2 | no | {4: 0, 11: 1} |
| 2 | 7 | 2 | no | {4: 0, 11: 1, 7: 2} |
| 3 | 2 | 7 | **yes, at 2** | return [2, 3] |

## Solution

```python
class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]:
        seen = {}  # number -> the index where we saw it

        for i in range(len(nums)):
            need = target - nums[i]   # the partner this number is missing

            if need in seen:          # one-step lookup in a dictionary
                return [seen[need], i]

            seen[nums[i]] = i         # remember this number for later ones

        return []  # never reached when an answer is guaranteed
```

Check for the partner **before** storing the current number. That way a number can't pair with itself.

## Complexity

- **Time:** O(n). One pass over the list; each dictionary check is O(1).
- **Space:** O(n). In the worst case `seen` holds every number.

The brute-force version (try every pair with two loops) is O(n²) time and O(1) space. The dictionary buys speed with memory. Saying that trade out loud is a good interview habit.

## Watch-outs

- Returning the numbers instead of their indexes.
- Storing before checking, which lets `[3]` with `target = 6` "pair" 3 with itself.
- Using `nums.index(need)` inside the loop: it hides a second loop and makes the solution O(n²).
