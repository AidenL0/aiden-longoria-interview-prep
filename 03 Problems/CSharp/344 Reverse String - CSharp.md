---
title: Reverse String
number: 344
language: csharp
pattern: two-pointers
difficulty: easy
tags: [two-pointers, in-place, arrays]
---

# 344 Reverse String (C#)

Pattern: [[Two Pointers]]

## What it asks

Reverse an array of characters **in place**: change the array you were given, using O(1) extra memory, and return nothing.

## Pattern and trigger

**Converging two pointers** that swap: the first and last trade places, then the second and second-to-last, and so on until they meet in the middle.

**Trigger:** "reverse" plus "in place."

## Picture

```
h  e  l  l  o
^           ^   swap → o e l l h
   ^     ^      swap → o l l e h
      ^         pointers meet: done
```

## Questions before coding

- Odd length? (The middle character stays where it is.)
- Return a new array? (No: modify `s` itself.)

## Trace table

`s = { 'h', 'e', 'l', 'l', 'o' }`

| left | right | swap | s after |
|---|---|---|---|
| 0 | 4 | h ↔ o | o e l l h |
| 1 | 3 | e ↔ l | o l l e h |
| 2 | 2 | — | loop ends (left is not < right) |

## Solution

```csharp
public class Solution {
    public void ReverseString(char[] s) {
        int left = 0;
        int right = s.Length - 1;

        while (left < right) {
            // tuple swap: both right-hand values are read before either is written
            (s[left], s[right]) = (s[right], s[left]);
            left++;
            right--;
        }
    }
}
```

The classic swap with a temporary variable does the same thing:

```csharp
char temp = s[left];
s[left] = s[right];
s[right] = temp;
```

## Complexity

- **Time:** O(n). n / 2 swaps.
- **Space:** O(1).

## Watch-outs

- `void` methods return nothing. LeetCode checks the array you changed.
- `Array.Reverse(s)` is one line and also correct, but an interviewer usually wants to see the two pointers.
- `while (left <= right)` also works (the middle swaps with itself), but `<` says what you mean.
