---
title: Fruit Into Baskets
number: 904
language: csharp
pattern: sliding-window
difficulty: medium
tags: [sliding-window, counting, dictionary]
status: worked-example
---

# 904 Fruit Into Baskets (C#)

Pattern: [[Sliding Window]] · Next: [[2747 Count Zero Request Servers - CSharp]]

> **Worked example.** A teaching walkthrough for a problem I'm still practicing, not my own solution. Try the problem yourself before reading on.

## What it asks

A row of trees: `fruits[i]` is the type of fruit on tree `i`. You have two baskets, and each basket holds only one type. Start at any tree and move right, picking one fruit from every tree, until a tree has a third type. What's the most fruit you can pick?

In plain terms: find the longest stretch of neighbors containing **at most two different values**.

## Pattern and trigger

**Sliding window with a counting board**: a window from `left` to `right` grows to the right, and a dictionary counts how many of each type are inside. When a third type gets in, shrink from the left until only two types remain.

**Trigger:** "longest stretch where some count stays under a limit."

## Picture

`fruits = { 1, 2, 3, 2, 2 }`

```
[1  2] 3  2  2      two types, length 2
[1  2  3]           three types → shrink from the left
 1 [2  3  2  2]     two types, length 4 ← best
```

## Questions before coding

- Can there be only one type? (Yes: the whole row is the answer.)
- Do the two types have to be different? (No, "at most two.")

## Trace table

`fruits = { 1, 2, 3, 2, 2 }`. `count.Count` is how many different types are in the window.

| right | fruit | count after adding | types | shrink? | left | best |
|---|---|---|---|---|---|---|
| 0 | 1 | {1:1} | 1 | no | 0 | 1 |
| 1 | 2 | {1:1, 2:1} | 2 | no | 0 | 2 |
| 2 | 3 | {1:1, 2:1, 3:1} | 3 | yes: remove 1 → {2:1, 3:1} | 1 | 2 |
| 3 | 2 | {2:2, 3:1} | 2 | no | 1 | 3 |
| 4 | 2 | {2:3, 3:1} | 2 | no | 1 | 4 |

## Solution

```csharp
public class Solution {
    public int TotalFruit(int[] fruits) {
        var count = new Dictionary<int, int>();   // fruit type -> how many in the window
        int left = 0;
        int best = 0;

        for (int right = 0; right < fruits.Length; right++) {
            // grow: add the new tree's fruit
            count[fruits[right]] = count.GetValueOrDefault(fruits[right]) + 1;

            // shrink: while there are three types, drop fruit from the left
            while (count.Count > 2) {
                int leftType = fruits[left];
                count[leftType]--;
                if (count[leftType] == 0) {
                    count.Remove(leftType);   // a zero row would still count as a type
                }
                left++;
            }

            best = Math.Max(best, right - left + 1);   // window length
        }
        return best;
    }
}
```

## Complexity

- **Time:** O(n). Each tree enters the window once and leaves at most once, even though there's a loop inside a loop.
- **Space:** O(1). The dictionary never holds more than three types.

## Watch-outs

- Forgetting `count.Remove` when a count hits 0. Nothing crashes; `count.Count` just overcounts and the answer comes out wrong (a silent bug).
- Window length is `right - left + 1`. Check it on a window of one tree.
