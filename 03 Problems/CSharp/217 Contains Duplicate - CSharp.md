---
title: Contains Duplicate
number: 217
language: csharp
pattern: hashing
difficulty: easy
tags: [arrays, hashing, hashset]
---

# 217 Contains Duplicate (C#)

Pattern: [[Hashing]] · Next: [[219 Contains Duplicate II - CSharp]]

## What it asks

Return `true` if any number appears at least twice in the array, and `false` if every number is different.

## Pattern and trigger

**Seen-before set.** A `HashSet<int>` holds each value once and answers "is it in here?" in one step.

**Trigger:** "has this appeared before?"

## Picture

```
nums: 1  2  3  1
set:  {1} {1,2} {1,2,3}  → 1 is already in the set → true
```

## Questions before coding

- Empty array or one element? (No duplicates possible: `false`.)
- Negative numbers? (Fine; a set doesn't care.)

## Trace table

`nums = { 1, 2, 3, 1 }`

| x | `seen.Add(x)` returns | seen after |
|---|---|---|
| 1 | true (new) | {1} |
| 2 | true | {1, 2} |
| 3 | true | {1, 2, 3} |
| 1 | **false** (already there) | return true |

## Solution

```csharp
public class Solution {
    public bool ContainsDuplicate(int[] nums) {
        var seen = new HashSet<int>();

        foreach (int x in nums) {
            // Add returns false when x is already in the set: that's the whole check
            if (!seen.Add(x)) {
                return true;
            }
        }
        return false;
    }
}
```

## Complexity

- **Time:** O(n). One pass; `Add` is O(1).
- **Space:** O(n) for the set.

**Other ways:** sort, then compare neighbors (O(n log n) time, O(1) extra space). Or compare every pair (O(n²)): too slow for large inputs.

## Watch-outs

- `HashSet.Add` doesn't throw on a duplicate; it returns `false`. (`Dictionary.Add` *does* throw.)
- Writing `seen.Contains(x)` and then `seen.Add(x)` works, but does the lookup twice.
