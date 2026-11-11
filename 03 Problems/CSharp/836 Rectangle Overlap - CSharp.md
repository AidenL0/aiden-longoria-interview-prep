---
title: Rectangle Overlap
number: 836
language: csharp
pattern: simulation-and-math
difficulty: easy
tags: [geometry, boolean-logic]
status: solved
---

# 836 Rectangle Overlap (C#)

Pattern: [[Simulation and Math]]

> **Solved.** I solved this one myself.

## What it asks

Each rectangle is given as `[x1, y1, x2, y2]`: its bottom-left corner `(x1, y1)` and top-right corner `(x2, y2)`. Its sides line up with the x and y axes. Return `true` if the two rectangles overlap with **positive area**. Touching only at an edge or a corner doesn't count.

## Pattern and trigger

**Reasoning about the opposite case.** Listing every way two rectangles *can* overlap is messy. Listing the ways they *can't* is easy: one is entirely to the left of, right of, above, or below the other. If **any one** of those is true, there's no overlap.

**Trigger:** "do these overlap?" Answer "are they separated?" and flip it.

## Picture

```
 ┌───┐
 │ 1 │ ┌───┐     rect1's right edge ≤ rect2's left edge
 └───┘ │ 2 │     → separated → no overlap
       └───┘
```

The four "separated" checks compare **edges** (single x or y values), not corners.

## Questions before coding

- Does sharing an edge count? (No: area must be positive, so use `<=` in the separated checks.)
- Can the rectangles be rotated? (No, the sides are axis-aligned.)

## Trace table

`rec1 = [0, 0, 2, 2]`, `rec2 = [1, 1, 3, 3]`

| Check | Values | Separated? |
|---|---|---|
| rec1 right ≤ rec2 left | 2 ≤ 1 | no |
| rec2 right ≤ rec1 left | 3 ≤ 0 | no |
| rec1 top ≤ rec2 bottom | 2 ≤ 1 | no |
| rec2 top ≤ rec1 bottom | 3 ≤ 0 | no |

No check says "separated," so they overlap: `true`.

## Solution

```csharp
public class Solution {
    public bool IsRectangleOverlap(int[] rec1, int[] rec2) {
        // name the indexes so the conditions read like the picture
        int left1 = rec1[0], bottom1 = rec1[1], right1 = rec1[2], top1 = rec1[3];
        int left2 = rec2[0], bottom2 = rec2[1], right2 = rec2[2], top2 = rec2[3];

        // any ONE of these is enough to keep them apart, so join with ||
        bool separated = right1 <= left2      // 1 is left of 2
                      || right2 <= left1      // 2 is left of 1
                      || top1 <= bottom2      // 1 is below 2
                      || top2 <= bottom1;     // 2 is below 1

        return !separated;
    }
}
```

## Complexity

- **Time:** O(1).
- **Space:** O(1).

## Where I got stuck

- I needed explanations for two things: thinking about edges instead of corners, and one logic flip.
- I wrote `&&` where I needed `||`. "Any one is enough" means `||`; "all must hold" means `&&`. Saying which one I mean before writing the condition helps.

## Watch-outs

- "Any one is enough" is `||`; "all must hold" is `&&`. Joining the four separated checks with `&&` makes the method say "overlap" almost every time. Say which you mean before writing the condition.
- Name the indexes. `rec1[2]` means nothing at a glance; `right1` does.
- Name a bool for what it means when it's `true`. A variable called `leftOverlap` that actually means "no overlap" invites a flipped answer.
