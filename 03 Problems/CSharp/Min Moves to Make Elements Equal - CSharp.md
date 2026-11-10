---
title: Min Moves to Make Elements Equal
number: unknown
language: csharp
pattern: simulation-and-math
difficulty: easy
tags: [math, arrays, max-loop]
---

# Min Moves to Make Elements Equal (C#)

Pattern: [[Simulation and Math]]

A LeetCode daily problem; its number wasn't recorded. (Don't confuse it with 453, where each move raises all but one element.)

## What it asks

You get an array of integers. One **move** adds 1 to a single element. Return the fewest moves that make every element equal.

## Pattern and trigger

**Math**: you can only raise numbers, so the smallest target you can reach is the current **maximum**. Every element has to climb up to it, and that climb is `max − value` moves.

**Trigger:** "only increase" plus "make all equal" → everyone meets at the max.

## Picture

`nums = { 2, 1, 3 }`, max = 3

```
2 → 3   1 move
1 → 3   2 moves
3 → 3   0 moves
        3 moves total
```

## Questions before coding

- Can moves lower a number? (No, only raise.)
- Negative numbers? (Possible in general. Start the max at `nums[0]`, not `0`.)

## Trace table

The code makes two passes, so there are two tables.

**Pass 1: find the max.** `biggest` starts at `nums[0]` = 2.

| value | value > biggest? | biggest after this step |
|---|---|---|
| 2 | no | 2 |
| 1 | no | 2 |
| 3 | yes | 3 |

**Pass 2: add up the gaps.** Every gap uses the final `biggest` = 3. `moves` starts at 0.

| value | gap = 3 − value | moves after this step |
|---|---|---|
| 2 | 1 | 1 |
| 1 | 2 | 3 |
| 3 | 0 | 3 |

## Solution

```csharp
public class Solution {
    public int MinMoves(int[] nums) {
        // pass 1: find the max with a loop (nums.Max() needs LINQ)
        int biggest = nums[0];          // works for negative numbers too
        foreach (int value in nums) {
            if (value > biggest) {
                biggest = value;
            }
        }

        // pass 2: every element climbs to the max
        int moves = 0;
        foreach (int value in nums) {
            moves += biggest - value;
        }
        return moves;
    }
}
```

## Complexity

- **Time:** O(n), two passes.
- **Space:** O(1).

## Watch-outs

- `.Max()`, `.Min()` and `.Sum()` are LINQ methods and need `using System.Linq;`. LeetCode adds it for you; some assessment editors don't (error CS1061). Writing the loop always works.
- Starting the max at `0` only works when every number is non-negative. Start at `nums[0]` or `int.MinValue`.
- For very large arrays the total could overflow an `int`; use `long` if the limits allow it.
