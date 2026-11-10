---
title: Contains Duplicate II
number: 219
language: csharp
pattern: sliding-window
difficulty: easy
tags: [sliding-window, hashset]
---

# 219 Contains Duplicate II (C#)

Pattern: [[Sliding Window]] · Before this: [[217 Contains Duplicate - CSharp]]

## What it asks

Return `true` if two **equal** numbers sit at most `k` positions apart: `nums[i] == nums[j]` and `|i − j| <= k`.

## Pattern and trigger

**Sliding window**: a frame of the last `k` numbers that slides right one step at a time. It's 217's seen-before set, except old numbers fall out of the set once they're too far back.

**Trigger:** "within distance k," "in the last k items."

## Picture

`nums = { 1, 2, 3, 1 }`, `k = 3`

```
i = 3: window holds the last 3 numbers before it
        [1  2  3] 1
         └──────┘ is 1 in here? yes → true
```

## Questions before coding

- Can `k` be 0? (Then no two different indexes qualify: always `false`.)
- Is the distance "at most k" or "less than k"? (At most: `<= k`.)

## Trace table

`nums = { 1, 2, 3, 1, 2, 3 }`, `k = 2`. The window keeps at most `k` numbers.

| i | nums[i] | in window? | window after adding | too big? remove nums[i − k] |
|---|---|---|---|---|
| 0 | 1 | no | {1} | no |
| 1 | 2 | no | {1, 2} | no |
| 2 | 3 | no | {1, 2, 3} | yes, remove nums[0] = 1 → {2, 3} |
| 3 | 1 | no | {2, 3, 1} | yes, remove nums[1] = 2 → {3, 1} |
| 4 | 2 | no | {3, 1, 2} | yes, remove nums[2] = 3 → {1, 2} |
| 5 | 3 | no | {1, 2, 3} | yes, remove nums[3] = 1 → {2, 3} |

Equal numbers are always 3 apart, more than `k = 2`, so the answer is `false`.

## Solution

```csharp
public class Solution {
    public bool ContainsNearbyDuplicate(int[] nums, int k) {
        var window = new HashSet<int>();   // the last k numbers

        for (int i = 0; i < nums.Length; i++) {
            if (window.Contains(nums[i])) {
                return true;               // an equal number within k steps
            }
            window.Add(nums[i]);

            if (window.Count > k) {        // the frame is too wide: drop the oldest
                window.Remove(nums[i - k]);
            }
        }
        return false;
    }
}
```

## Complexity

- **Time:** O(n). Each number is added once and removed at most once.
- **Space:** O(min(n, k)) for the window.

## Watch-outs

- The oldest number in the window is `nums[i - k]`, not `nums[i - k - 1]`. Check it on the trace table.
- The window can't hold duplicates by the time you add (you'd have returned `true`), so `Count` really is the window's width.
