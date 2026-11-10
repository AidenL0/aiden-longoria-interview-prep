---
title: Two Sum
number: 1
language: csharp
pattern: hashing
difficulty: easy
tags: [arrays, hashing, dictionary]
---

# 1 Two Sum (C#)

Other language: [[1 Two Sum - Python]] · Pattern: [[Hashing]] · Syntax help: [[CSharp for Python Programmers]]

## What it asks

You get an array of integers `nums` and an integer `target`. Exactly two numbers add up to `target`. Return their **indexes** (positions, counting from 0).

## Pattern and trigger

**Hashing** with a `Dictionary<int, int>` (number → index). For each number, its partner is `target - number`. Remember what you've seen so the partner check is one step.

**Trigger:** "two things that combine to a target."

## Picture

`nums = { 4, 11, 7, 2 }`, `target = 9`

```
index:  0   1   2   3
value:  4  11   7   2
                ^   ^   7 + 2 = 9 → { 2, 3 }
```

## Questions before coding

- Same element twice? (No.)
- Always exactly one answer? (Yes.)
- What should the method return if nothing matches? (The return type is `int[]`, so `new int[0]`, never `-1`.)

## Trace table

| i | nums[i] | need | `TryGetValue(need, out int j)` | seen after |
|---|---|---|---|---|
| 0 | 4 | 5 | false | {4: 0} |
| 1 | 11 | −2 | false | {4: 0, 11: 1} |
| 2 | 7 | 2 | false | {4: 0, 11: 1, 7: 2} |
| 3 | 2 | 7 | **true, j = 2** | return { 2, 3 } |

## Solution

```csharp
public class Solution {
    public int[] TwoSum(int[] nums, int target) {
        var seen = new Dictionary<int, int>();   // number -> index

        for (int i = 0; i < nums.Length; i++) {  // arrays use .Length
            int need = target - nums[i];

            // checks the key and reads its value in one step
            if (seen.TryGetValue(need, out int j)) {
                return new int[] { j, i };
            }

            seen[nums[i]] = i;   // adds the key, or overwrites it if present
        }

        return new int[0];   // every path must return an int[]
    }
}
```

## Complexity

- **Time:** O(n). One pass; dictionary lookups are O(1).
- **Space:** O(n) for the dictionary.

## Watch-outs

These all happened while learning this problem:

- `foreach` has no index. Use `for`, or declare a counter **before** the loop (inside it, it resets every pass).
- A `return` inside the loop body without a condition ends the method on the first pass.
- `return -1;` won't compile in a method that returns `int[]` (error CS0029).
- `TryGetValue` on a missing key returns `false` and sets the `out` value to 0. A wrong key fails silently, and a test can still pass by luck when the right answer happens to be 0.
- `seen.Add(key, value)` throws on a duplicate key; `seen[key] = value` doesn't.
- Write `out int j`: `out` first, then the type, then the name.
- Declare before use: `int need = ...`, not `need = ...` (CS0103).
